```TypeScript
/**
 * No propagation needs to take place on a resource; propagation only happens when
 * changing policies on a container.
 *
 * agent    : The Agent issuing the PUT request to overwrite the ACR associated with the Resource
 * resource : The Resource associated with the ACR to be overwritten
 * newAcr   : The access control statements to overwrite the ACR with
 *
 * Return    : 403 or 401 if the Agent does not have permission.
 *             204 if the ACR is overwritten successfully.
 */
//@Transactional
export function putResourceAcr(agent: Agent, resource: Resource, newAcr: ACR) {
  let removeAcr: ACR;
  let addAcr: ACR;
  [removeAcr, addAcr] = ACR.diff(resource.getAccessControlResource(), newAcr);
  if (!canChangeAcr(agent, resource, removeAcr, addAcr)) return accessError(agent);

  return doPutResourceAcr(agent, resource, newAcr);
}

function doPutResourceAcr(agent: Agent, resource: Resource, newAcr: ACR) {
  const acr = resource.getAccessControlResource();

  // Handle propagation of the removal and adding of policies
  acr.removeStatements();
  acr.addStatements(newAcr);

  return StatusCodes.NO_CONTENT;
}

/**
 * No propagation needs to take place on a resource; propagation only happens when
 * changing policies on a container.
 *
 * agent     : The Agent issuing the PATCH request to update the ACR associated with the Resource
 * resource  : The Resource associated with the ACR to be updated
 * removeAcr : The access control statements to remove from  the ACR
 * addAcr    : The access control statements to add to the ACR
 *
 * Return    : 403 or 401 if the Agent does not have permission.
 *             204 if the ACR is updated successfully.
 */
//@Transactional
export function patchResourceAcr(agent: Agent, resource: Resource, removeAcr: ACR, addAcr: ACR) {
  if (!canChangeAcr(agent, resource, removeAcr, addAcr)) return accessError(agent);

  return doPatchResourceAcr(agent, resource, removeAcr, addAcr);
}

function doPatchResourceAcr(agent: Agent, resource: Resource, removeAcr: ACR, addAcr: ACR) {
  const acr = resource.getAccessControlResource();

  acr.removeStatements(removeAcr);
  acr.addStatements(addAcr);

  return StatusCodes.NO_CONTENT;
}

/**
 * agent     : The Agent issuing the PUT request to overwrite the ACR associated with the Container
 * container : The Container associated with the ACR to be overwritten
 * newAcr    : The content to overwrite the ACR with
 *
 * Return    : 403 or 401 if the Agent does not have permission.
 *             204 if the ACR is overwritten successfully.
 */
//@Transactional
export function putContainerAcr(agent: Agent, container: Container, newAcr: ACR) {
  let removeAcr: ACR;
  let addAcr: ACR;
  [removeAcr, addAcr] = ACR.diff(container.getAccessControlResource(), newAcr);
  if (!canChangeContainerAcr(agent, container, removeAcr, addAcr)) return accessError(agent);

  return doPatchContainerAcr(agent, container, removeAcr, addAcr);
}

/**
 * agent     : The Agent issuing the PATCH request to update the ACR associated with the Container
 * container : The Container associated with the ACR to be updated
 * removeAcr : The access control statements to remove from  the ACR
 * addAcr    : The access control statements to add to the ACR
 *
 * Return    : 403 or 401 if the Agent does not have permission.
 *             204 if the ACR is overwritten successfully.
 */
//@Transactional
export function patchContainerAcr(agent: Agent, container: Container, removeAcr: ACR, addAcr: ACR) {
  if (!canChangeContainerAcr(agent, container, removeAcr, addAcr)) return accessError(agent);

  return doPatchContainerAcr(agent, container, removeAcr, addAcr);
}

function doPatchContainerAcr(agent: Agent, container: Container, removeAcr: ACR, addAcr: ACR) {
  const acr = container.getAccessControlResource();

  acr.removeStatements(removeAcr);
  acr.addStatements(addAcr);
  copyMemberStatementsToNonMemberStatements(removeAcr);
  copyMemberStatementsToNonMemberStatements(addAcr);

  container.children().forEach((resource: Resource) => {
    if (resource.isContainer()) {
      doPatchContainerAcr(agent, <Container>resource, removeAcr, addAcr);
    } else {
      doPatchResourceAcr(agent, resource, removeAcr, addAcr);
    }
  });

  return StatusCodes.NO_CONTENT;
}

///////////////////// Helper functions /////////////////////

/**
 * When propagating policies from a Container to its children
 *
 * Container                     Child
 * acp:accessMembers          -> acp:access
 * acp:accessMembers          -> acp:accessMembers
 * acp:accessMembersProtected -> acp:accessProtected
 * acp:accessMembersProtected -> acp:accessMembersProtected
 * acp:accessMembersLocked    -> acp:accessLocked
 * acp:accessMembersLocked    -> acp:accessMembersLocked
 * acp:applyMembers           -> acp:apply
 * acp:applyMembers           -> acp:applyMembers
 * acp:applyMembersProtected  -> acp:applyProtected
 * acp:applyMembersProtected  -> acp:applyMembersProtected
 * acp:applyMembersLocked     -> acp:applyLocked
 * acp:applyMembersLocked     -> acp:applyMembersLocked
 *
 * After copying the statements from the new ACR to the ACR
 * associated with the Container, we then modify the statements
 * in the new ACR so that the statements copied to all children
 * are correct as per the table above.
 *
 * The same applies when removing statements. If a 'Members'
 * statement is removed from a container then the associated
 * statements that were originally propagated to the container's
 * children need to be removed also. For example, when a Policy
 * was added to a container using acp:applyMembers, then when
 * that Policy is removed from the container, the associated
 * acp:apply and acp:applyMembers statements for that Policy
 * in the Container's children need to be removed.
 */
function copyMemberStatementsToNonMemberStatements(acr: ACR) {
  // Modify acr by copying the statements as per the table
  // in the function comment above
}

/**
 * Check the agent has permissions to remove and add all the relevant
 * Policies.
 * The relevant predicats are acp:accessXXX and acp:applyXXX
 *
 * agent     : The agent trying to change the ACR
 * resource  : The Resource associated with the ACR being changed
 * removeAcr : The Policies being removed from the ACR
 * addAcr    : The Policies being added to the ACR
 */
function canChangeAcr(agent: Agent, resource: Resource, removeAcr: ACR, addAcr: ACR): boolean {
  if (!canChangeAccessAcr(agent, resource, removeAcr, addAcr)) return false;
  if (!canRemovePoliciesAcr(agent, resource, removeAcr)) return false;
  if (!canAddPoliciesAcr(agent, resource, addAcr)) return false;

  return true;
}

/**
 * Check the agent has permissions to remove and add all the relevant
 * Policies from a container and its children. The children are
 * relevant when any of the 'Members' (e.g.applyMembers)
 * predicates are used as this would have resuled in propagation
 * to children when this statements were initially added.
 *
 * The relevant predicats are acp:accessXXX and acp:applyXXX
 *
 * agent     : The agent trying to change the ACR
 * resource  : The Resource associated with the ACR being changed
 * removeAcr : The Policies being removed from the ACR
 * addAcr    : The Policies being added to the ACR
 */
function canChangeContainerAcr(agent: Agent, container: Container, removeAcr: ACR, addAcr: ACR): boolean {
  if (!canChangeAccessAcr(agent, container, removeAcr, addAcr)) return false;
  if (!canRemovePoliciesAcr(agent, container, removeAcr)) return false;
  if (!canAddPoliciesAcr(agent, container, addAcr)) return false;

  return container.children().every((resource: Resource) => {
    if (resource.isContainer()) {
      if (!canChangeContainerAcr(agent, <Container>resource, removeAcr, addAcr)) return false;
    } else {
      if (!canChangeAcr(agent, resource, removeAcr, addAcr)) return false;
    }

    return true;
  });
}

/**
 * can change the acp:accessXXX statements
 */
function canChangeAccessAcr(agent: Agent, resource: Resource, removeAcr: ACR, addAcr: ACR): boolean {
  if (!canChangeAccess(agent, resource, removeAcr, addAcr)) return false;
  if (!canChangeAccessProtected(agent, resource, removeAcr, addAcr)) return false;
  if (!canChangeAccessLocked(agent, resource, removeAcr, addAcr)) return false;

  return true;
}

/**
 * can remove the relevant acp:applyXXX statements
 */
function canRemovePoliciesAcr(agent: Agent, resource: Resource, removeAcr: ACR): boolean {
  if (!canRemovePolicies(agent, resource, removeAcr)) return false;
  if (!canRemoveProtectedPolicies(agent, resource, removeAcr)) return false;
  if (!canRemoveLockedPolicies(agent, resource, removeAcr)) return false;

  return true;
}

/**
 * can add the relevant acp:applyXXX statements
 */
function canAddPoliciesAcr(agent: Agent, resource: Resource, addAcr: ACR): boolean {
  if (!canAddPolicies(agent, resource, addAcr)) return false;
  if (!canAddProtectedPolicies(agent, resource, addAcr)) return false;
  if (!canAddLockedPolicies(agent, resource, addAcr)) return false;

  return true;
}

/**
 * acp:access
 */
function canChangeAccess(agent: Agent, resource: Resource, removeAcr: ACR, addAcr: ACR): boolean {
  if (removeAcr.access.length || addAcr.access.length) {
    // need Write access. PUT will map to Write access
    if (!isAcrAccessAllowed(agent, resource, HTTPMethod.PUT)) return false;
  }

  if (resource.isContainer())
    if (removeAcr.accessMembers.length || addAcr.accessMembers.length)
      return isAcrAccessMembersAllowed(agent, resource, HTTPMethod.PUT);

  return true;
}

/**
 * acp:accessProtected
 */
function canChangeAccessProtected(agent: Agent, resource: Resource, removeAcr: ACR, addAcr: ACR): boolean {
  if (removeAcr.accessProtected.length || addAcr.accessProtected.length)
    if (!isAcrAccessProtectedAllowed(agent, resource, HTTPMethod.PUT)) return false;

  if (resource.isContainer())
    if (removeAcr.accessMembersProtected.length || addAcr.accessMembersProtected.length)
      return isAcrAccessMembersProtectedAllowed(agent, resource, HTTPMethod.PUT);

  return true;
}

/**
 * acp:accessLocked
 */
function canChangeAccessLocked(agent: Agent, resource: Resource, removeAcr: ACR, addAcr: ACR): boolean {
  if (removeAcr.accessLocked.length || addAcr.accessLocked.length)
    return isAcrAccessLockedAllowed(agent, resource, HTTPMethod.PUT);

  if (resource.isContainer())
    if (removeAcr.accessMembersLocked.length || addAcr.accessMembersLocked.length)
      return isAcrAccessMembersLockedAllowed(agent, resource, HTTPMethod.PUT);

  return true;
}

/**
 * can remove acp:apply statements
 */
function canRemovePolicies(agent: Agent, resource: Resource, removeAcr: ACR): boolean {
  return canChangeAccess(agent, resource, removeAcr, new ACR());
}

/**
 * can remove acp:applyProtected statements
 */
function canRemoveProtectedPolicies(agent: Agent, resource: Resource, removeAcr: ACR): boolean {
  if (!canChangeAccessProtected(agent, resource, removeAcr, new ACR())) return false;

  //check that each individual protected policy can be removed by this agent
  if (
    !removeAcr.getAccessControls().every((ac: AccessControl) => {
      ac.applyProtected.every((p: Policy) => {
        const container: Container = getOriginContainerForPolicy(p);
        if (!isAcrAccessProtectedAllowed(agent, container, HTTPMethod.PUT)) return false;
        return true;
      });
    })
  )
    return false;

  if (resource.isContainer()) {
    return removeAcr.getAccessControls().every((ac: AccessControl) => {
      ac.applyMembersProtected.every((p: Policy) => {
        const container: Container = getOriginContainerForPolicy(p);
        if (!isAcrAccessProtectedAllowed(agent, container, HTTPMethod.PUT)) return false;
        return true;
      });
    });
  }
  return true;
}

function getOriginContainerForPolicy(policy: Policy): Container {
  // The Solid Server is responsible for maintaining the Iri for
  // the Container from which all protected policies originated.
  // If a policy is added with acp:applyProtected or
  // acp:applyMembersProtected then the origin will be the
  // parent Container.
  // If a policy is propagated via acp:applyMembersProtected
  // then the origin will be the Container where the
  // acp:applyMembersProtected statement was initially added.
  //
  // e.g if a protected policy is added to /A/B using
  // acp:applyMembersProtected then the origin is /A.
  // If that policy is later propagated to /A/B/C/D then
  // the origin for the protected policy at /A/B/C/D is
  // still /A

  return new Container();
}
/**
 * can remove acp:applyLocked statements
 */
function canRemoveLockedPolicies(agent: Agent, resource: Resource, removeAcr: ACR): boolean {
  if (!canChangeAccessLocked(agent, resource, removeAcr, new ACR())) return false;

  // Removing a locked policy request write access from acp:accessLocked
  // on the root container
  if (!isAcrAccessLockedAllowed(agent, getRootContainer(), HTTPMethod.PUT)) return false;

  return true;
}

function getRootContainer(): Container {
  // The server will be able to provide the reference for the root
  // container for this Pod
  return new Container();
}

/**
 * can add acp:apply statements
 */
function canAddPolicies(agent: Agent, resource: Resource, addAcr: ACR): boolean {
  return canChangeAccess(agent, resource, new ACR(), addAcr);
}

/**
 * can add acp:applyProtected statements
 */
function canAddProtectedPolicies(agent: Agent, resource: Resource, addAcr: ACR): boolean {
  return canChangeAccessProtected(agent, resource, new ACR(), addAcr);
}

/*
 * can add acp:applyLocked statements
 */
function canAddLockedPolicies(agent: Agent, resource: Resource, addAcr: ACR): boolean {
  return canChangeAccessLocked(agent, resource, new ACR(), addAcr);
}
```
