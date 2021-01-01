```TypeScript
////////////////// RESOURCES //////////////////

/**
 * Note this this function creates a Resource and therefore there is no
 * Policy propagation required. Propagation only occurs when Policy changees
 * happen on a Container.
 *
 * agent     : The Agent trying to PUT the resource
 * resource  : The Resource the Agent is trying to PUT
 * container : The parent Container for the Resource
 * content   : The content for the Resource the Agent wants to PUT in the Container
 *
 * Return    : 403 or 401 if the Agent does not have permission.
 *             204 if the resource already exists and if overwritten successfully.
 *             201 if the resource did not already exists and is created.
 */
export function putResource(agent: Agent, resource: Resource, container: Container, content: Readable) {
  if (resource.exists()) {
    if (!isAccessAllowed(agent, resource, HTTPMethod.PUT)) return accessError(agent);

    resource.write(content);
    return StatusCodes.NO_CONTENT;
  }

  if (!isAccessAllowed(agent, container, HTTPMethod.PUT)) return accessError(agent);

  createResource(container, resource.getIri(), content);

  return StatusCodes.CREATED;
}

/**
 * When an Agent sends a POST request to a container,
 * a new child Resource will be created in the container.
 *
 * Note this this function creates a Resource and therefore there is no
 * Policy propagation required. Propagation only occurs when Policy changees
 * happen on a Container.
 *
 * agent     : The Agent trying to POST to the Container
 * container : The Container the Agent sent the request to
 * slug      : The hint provided by the requestor for the child resource name
 * content   : The content for the Resource the Agent wants to POST to the Container
 *
 * Return    : 403 or 401 if the Agent does not have permission.
 *             201 if the resource is created.
 */
export function postResource(agent: Agent, container: Container, slug: string, content: Readable) {
  if (!isAccessAllowed(agent, container, HTTPMethod.POST)) return accessError(agent);

  const iri: IRI = container.newResourceName(slug);
  createResource(container, iri, content);

  return StatusCodes.CREATED;
}

/**
 * Note this this function updates a Resource and therefore there is no
 * Policy propagation required. Propagation only occurs when Policy changees
 * happen on a Container.
 *
 * If the PATCH only contains content to be appended (e.g. triples to add)
 * then acp:Append access is requred.
 * However, if the PATCH contains content that requires changes
 * (e.g. triples to be removed) then acp:Write access is required.
 *
 * agent    : The Agent tryping to PATH the resource.
 * resource : The Resource the Agent is trying to PATCH
 * content  : The content the Agent is using to PATCH the Resource. This pseudo-code does
 *            not attemptto describe how the patch operation is carried out.
 *
 * Return   : 403 or 401 if the Agent does not have permission.
 *            204 if the resource is patched successfully.
 *
 */
export function patchResource(agent: Agent, resource: Resource, content: Readable, appendOnly: boolean) {
  if (appendOnly) {
    if (!isAccessAllowed(agent, resource, HTTPMethod.PATCH)) return accessError(agent);
  } else {
    if (!isAccessAllowed(agent, resource, HTTPMethod.PUT)) return accessError(agent);
  }
  if (!resource.exists()) return StatusCodes.NOT_FOUND;

  resource.applyPatch(content);

  return StatusCodes.NO_CONTENT;
}

/**
 * Delete a resource and the associated ACR.
 *
 * NOTE : FOR DISCUSSION. I think it would be better if we had a specific DELETE
 *        access rather than overloading the WRITE access. This would be on the
 *        resource directly rather than on the parent container. By overloading the
 *        Write access we are assuming there are no use cases where somebody should not
 *        be able to delete a resource even if they can remove the content. This
 *        would also map much cleaner to the HTTP methods.
 *        Deleting a resource does modify the containment triples in the container
 *        but I don't think that is a good reason to require acp:write access on
 *        the container because those triples are server managed and cannot be
 *        manipulated directly. Even if the decision was to still require acp:write
 *        access on the container becuase it is changing, I still think it is a valid
 *        case to have acp:delete to control deletion of the resource.
 */
export function deleteResource(agent: Agent, resource: Resource, container: Container) {
  if (!isAccessAllowed(agent, resource, HTTPMethod.DELETE)) return accessError(agent);
  if (!resource.exists()) return StatusCodes.NOT_FOUND;

  container.delete(resource);
  container.deleteACR(resource);
  container.deleteContainmentTriples(resource);

  return StatusCodes.NO_CONTENT;
}

////////////////// CONTAINERS //////////////////

/**
 * agent      : The Agent trying to PUT the container
 * container  : The Container the Agent is trying to PUT
 * pContainer : The parent Container for the container
 * content    : The content for the Container the Agent wants to PUT in the parent Container.
 *              The content cannot includecontainment triples but can be any triples that are
 *              valid in a normal Resource.
 *
 *              FOR DISCUSSION. If containment triples exist should they be ignored
 *              or should it result in an error. This dicision may already have been made
 *              in the LDP spec???
 *
 * Return     : 403 or 401 if the Agent does not have permission.
 *              204 if the container already exists and the non containment triples have
 *              been overwritten successfully.
 *              201 if the container did not already exist and has been created.
 */
export function putContainer(agent: Agent, container: Container, pContainer: Container, content: Readable) {
  if (container.exists()) {
    if (!isAccessAllowed(agent, container, HTTPMethod.PUT)) return accessError(agent);

    // non containment triples
    container.write(content);

    return StatusCodes.NO_CONTENT;
  }

  if (!isAccessAllowed(agent, pContainer, HTTPMethod.PUT)) return accessError(agent);

  createContainer(pContainer, container.getIri(), content);

  return StatusCodes.CREATED;
}

/**
 * agent     : The Agent trying to POST to the Container
 * container : The Container the Agent sent the request to
 * slug      : The hint provided by the requestor for the child container name
 * content   : The content for the Resource the Agent wants to POST to the Container
 *
 * Return    : 403 or 401 if the Agent does not have permission.
 *             201 if the child container is created.
 */
export function postContainer(agent: Agent, container: Container, slug: string, content: Readable) {
  if (!isAccessAllowed(agent, container, HTTPMethod.POST)) return accessError(agent);

  const iri: IRI = container.newResourceName(slug);
  createContainer(container, iri, content);

  return StatusCodes.CREATED;
}

/**
 * agent     : The Agent tryping to PATH the container.
 * container : The Container the Agent is trying to PATCH
 * content   : The content the Agent is using to PATCH the container. This pseudo-code does
 *             not attemptto describe how the patch operation are carried out.
 *
 * Return   : 403 or 401 if the Agent does not have permission.
 *            204 if the container is patched successfully.
 *
 */
export function patchContainer(agent: Agent, container: Container, content: Readable) {
  if (!isAccessAllowed(agent, container, HTTPMethod.PATCH)) return accessError(agent);
  if (!container.exists()) return StatusCodes.NOT_FOUND;

  // non containment triples
  container.applyPatch(content);

  return StatusCodes.NO_CONTENT;
}

/**
 *
 * When an Agent tries to delete a Container in the parent Container
 * with the DELETE HTTP method, the following algorithms are used to
 * enforce access control and policy propagation.
 *
 * There are 4 approaches available :
 *
 * 1. Delete the container only if it is empty
 * 2. Delete the container and its children. If any child cannot be
 *    deleted then continue to the next child. Return a list of the
 *    children that could not be deleted. This is similar to how most
 *    file systems work.
 * 3. Delete the container and its children. All children are deleted
 *    or the operation fails. This requires transactional support.
 * 4. Delete the container and its children. If the agent has permission
 *    to delete the container, then the permissions for the children
 *    are ignored and the children are deleted.
 *
 * In addition, the delete operation can be carried out synchronously or
 * asynchronously. If the client asks for the operation to be carried
 * out asynchronously and the server is capable of an asynchronous
 * operation then the response will be 202 (ACCEPTED).
 * When the operation is complete the server will send a notification
 * to the parent container along with the result of the operation.
 * This has not been implemented yet so there is no work done on the
 * structure of the notification or result.
 *
 * NOTE: We have not done any work on how a client would negotiate
 *       with the server on which delete mechanism to use.
 *       We could use a mechanism similar to content negotiation.
 */

/**
 * Delete a container is it is empty.
 *
 * agent      : The Agent trying to delete the container
 * container  : The Container to delete
 * pContainer : The parent Container of the Container to delete
 *
 * Return     : 403 or 401 if the Agent does not have permission.
 *              403 if the container is not empty.
 *              204 if the container is successfully deleted.
 */
export function deleteContainerIfEmpty(agent: Agent, container: Container, pContainer: Container) {
  if (!isAccessAllowed(agent, container, HTTPMethod.DELETE)) return accessError(agent);

  if (!container.exists()) return StatusCodes.NOT_FOUND;

  if (!container.isEmpty()) return StatusCodes.FORBIDDEN;

  pContainer.deleteContainer(container);
  pContainer.deleteACR(container);
  pContainer.deleteContainmentTriples(container);

  return StatusCodes.NO_CONTENT;
}

/**
 * Delete a container and all the children in the container.
 * This is not an atomic function.
 *
 * agent      : The Agent trying to delete the container
 * container  : The Container to delete
 * pContainer : The parent Container of the Container to delete
 *
 * Return     : 403 or 401 if the Agent does not have permission.
 *
 *              FOR DISCUSSION: Is 200 the appropriate error code?
 *              200 if some of the children could be deleted and a list of
 *              the IRIs that could not be deleted.
 *
 *              204 if all the children could be deleted.
 */
export function deleteContainerNonAtomic(
  agent: Agent,
  container: Container,
  pContainer: Container
): [StatusCodes, IRI[]] {
  if (!isAccessAllowed(agent, container, HTTPMethod.DELETE)) return [accessError(agent), [container.getIri()]];

  if (!container.exists()) return [StatusCodes.NOT_FOUND, [container.getIri()]];

  const cannotDelete: IRI[] = [];

  container.children().forEach((resource: Resource) => {
    if (resource.isContainer()) {
      let fails: IRI[];
      [, fails] = deleteContainerNonAtomic(agent, <Container>resource, container);
      cannotDelete.concat(fails);
    } else {
      if (deleteResource(agent, resource, container) !== StatusCodes.NO_CONTENT) cannotDelete.push(resource.getIri());
    }
  });

  if (container.isEmpty()) {
    pContainer.deleteContainer(container);
    pContainer.deleteACR(container);
    pContainer.deleteContainmentTriples(container);

    return [StatusCodes.NO_CONTENT, cannotDelete];
  } else {
    cannotDelete.push(container.getIri());

    return [StatusCodes.OK, cannotDelete];
  }
}

/**
 * Delete a container recursively. This is an atomic operation.
 *
 * agent      : The Agent trying to delete the container
 * container  : The Container to delete
 * pContainer : The parent Container of the Container to delete
 *
 * Return     : 403 or 401 if the Agent does not have permission.
 *              204 if the container is successfully deleted.
 */
//@Transactional
export function deleteContainerAtomic(agent: Agent, container: Container, pContainer: Container) {
  if (!isAccessAllowed(agent, container, HTTPMethod.DELETE)) return accessError(agent);

  const result: boolean = container.children().every((resource: Resource) => {
    if (resource.isContainer()) {
      if (deleteContainerAtomic(agent, <Container>resource, container) !== StatusCodes.NO_CONTENT) return false;
    } else {
      // not a container
      if (deleteResource(agent, resource, container) !== StatusCodes.NO_CONTENT) return false;
    }
    return true;
  });
  if (!result) return accessError(agent);

  pContainer.deleteContainer(container);
  pContainer.deleteACR(container);
  pContainer.deleteContainmentTriples(container);

  return StatusCodes.NO_CONTENT;
}

/**
 * Delete a container recursively but do not check permissions recursively.
 * Permission for DELETE access is only checked on the initial container.
 *
 * agent      : The Agent trying to delete the container
 * container  : The Container to delete
 * pContainer : The parent Container of the Container to delete
 *
 * Return     : 403 or 401 if the Agent does not have permission.
 *              204 if the container is successfully deleted.
 */
export function deleteContainerNonRecursivePermissions(agent: Agent, container: Container, pContainer: Container) {
  if (!isAccessAllowed(agent, container, HTTPMethod.DELETE)) return accessError(agent);

  deleteContainerIgnorePermissions(agent, container, pContainer);

  return StatusCodes.NO_CONTENT;

  function deleteContainerIgnorePermissions(agent: Agent, container: Container, pContainer: Container) {
    container.children().forEach((resource: Resource) => {
      if (resource.isContainer()) deleteContainerIgnorePermissions(agent, <Container>resource, container);
      else {
        container.delete(resource);
        container.deleteACR(resource);
        container.deleteContainmentTriples(resource);
      }
    });

    pContainer.deleteContainer(container);
    pContainer.deleteACR(container);
    pContainer.deleteContainmentTriples(container);
  }
}

////////////////// HELPER FUNCTIONS //////////////////

/**
 * This function is called when an agent is attempting to access a resource but
 * does not have permission.
 *
 * agent : The agent attempting to access the resource
 *
 * Return: 403 if the agent is logged in, 401 if the agent is not logged in.
 */
export function accessError(agent: Agent) {
  if (agent.isAuthenticated()) return StatusCodes.FORBIDDEN;

  return StatusCodes.UNAUTHORIZED;
}

/**
 * Create a new resource in a container. Propagate all the relevant polcies
 * from the parent container ACR to the new resource ACR.
 *
 * container : The Container in which the resource will be created
 * iri       : The IRI for the new resource. This must be a child of the container
 * content   : The content to write to the new resource
 */
//@Transactional
function createResource(container: Container, iri: IRI, content: Readable) {
  const resource: Resource = container.createResource(iri);
  container.createACR(iri); // Create ACR for resource at iri
  container.updateContainmentTriples(resource);
  if (content) resource.write(content);
  propagatePolicies(container, resource);
}

/**
 * Create a new container in an existing container. Propagate all the relevant polcies
 * from the parent container ACR to the new container ACR.
 *
 * container : The Container in which the new container  will be created
 * iri       : The IRI for the new container. This must be a child of the container
 * content   : The content to write to the new container. Note this cannot include
 *             containment triples.
 */
//@Transactional
function createContainer(pContainer: Container, iri: IRI, content: Readable) {
  const container: Container = pContainer.createContainer(iri);
  pContainer.createACR(iri); // Create ACR for container at iri
  pContainer.updateContainmentTriples(container);
  // write the non containment triples
  if (content) container.write(content);
  propagatePoliciesToContainer(pContainer, container);
}

/**
 * Propagate policies from the container ACR to the child resource ACR.
 * Container                     Child Resource
 * acp:accessMembers          -> acp:access
 * acp:accessMembersProtected -> acp:accessMembers
 * acp:accessMembersLocked    -> acp:accessLocked
 * acp:applyMembers           -> acp:apply
 * acp:applyMembersProtected  -> acp:applyProtected
 * acp:applyMembersLocked     -> acp:applyLocked
 */
function propagatePolicies(container: Container, resource: Resource) {
  const acr = resource.getAccessControlResource();
  const cacr = container.getAccessControlResource();

  propagate(cacr, cacr.getIri(), ACP.ACCESS_MEMBERS, acr, ACP.ACCESS);
  propagate(cacr, cacr.getIri(), ACP.ACCESS_MEMBERS_PROTECTED, acr, ACP.ACCESS_PROTECTED);
  propagate(cacr, cacr.getIri(), ACP.ACCESS_MEMBERS_LOCKED, acr, ACP.ACCESS_LOCKED);

  cacr.getAccessControls().forEach((ac: AccessControl) => {
    propagate(cacr, ac.getIri(), ACP.APPLY_MEMBERS, acr, ACP.APPLY);
    propagate(cacr, ac.getIri(), ACP.APPLY_MEMBERS_PROTECTED, acr, ACP.APPLY_PROTECTED);
    propagate(cacr, ac.getIri(), ACP.APPLY_MEMBERS_LOCKED, acr, ACP.APPLY_LOCKED);
  });

  return;
}

/**
 * Propagate policies from the parent container ACR to the
 * child container ACR.
 *
 * Parent Container              Child Container
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
 */
function propagatePoliciesToContainer(pContainer: Container, container: Container) {
  const pAcr = pContainer.getAccessControlResource();
  const cAcr = container.getAccessControlResource();

  propagate(pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS, cAcr, ACP.ACCESS);
  propagate(pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS, cAcr, ACP.ACCESS_MEMBERS);
  propagate(pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS_PROTECTED, cAcr, ACP.ACCESS_PROTECTED);
  propagate(pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS_PROTECTED, cAcr, ACP.ACCESS_MEMBERS_PROTECTED);
  propagate(pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS_LOCKED, cAcr, ACP.ACCESS_LOCKED);
  propagate(pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS_LOCKED, cAcr, ACP.ACCESS_MEMBERS_LOCKED);

  pAcr.getAccessControls().forEach((ac: AccessControl) => {
    propagate(pAcr, ac.getIri(), ACP.APPLY_MEMBERS, cAcr, ACP.APPLY);
    propagate(pAcr, ac.getIri(), ACP.APPLY_MEMBERS, cAcr, ACP.APPLY_MEMBERS);
    propagate(pAcr, ac.getIri(), ACP.APPLY_MEMBERS_PROTECTED, cAcr, ACP.APPLY_PROTECTED);
    propagate(pAcr, ac.getIri(), ACP.APPLY_MEMBERS_PROTECTED, cAcr, ACP.APPLY_MEMBERS_PROTECTED);
    propagate(pAcr, ac.getIri(), ACP.APPLY_MEMBERS_LOCKED, cAcr, ACP.APPLY_LOCKED);
    propagate(pAcr, ac.getIri(), ACP.APPLY_MEMBERS_LOCKED, cAcr, ACP.APPLY_MEMBERS_LOCKED);
  });
}

/**
 * Find statements matching (csubject, cpredicate, *) in the container ACR
 * and write those statements to the resource ACR after replacing the predicate with rpredicate.
 * (csubject, cpredicate, *) -> (csubject, rpredicate, *)
 *
 * cacr       : The ACR for the Container
 * csubject   : The subject of the triples in the container ACR
 * cpredicate : The predicate of the triples in the container ACR
 * acr        : The ACR for the Resource
 * rpredicate : The predicate to use for the triples written to the Resource
 */
function propagate(cacr: ACR, csubject: IRI, cpredicate: IRI, acr: ACR, rpredicate: IRI) {
  const statements: Statement[] = findStatements(cacr, csubject, cpredicate);

  // The base IRI for the subject would also need to be replaced if absolute IRIs are used.
  // so that the Resource IRI rather than the Container IRI is used.
  const resourceStatements: Statement[] = replacePredicate(statements, cpredicate, rpredicate);

  acr.addStatements(resourceStatements);

  return;
}

function findStatements(acr: ACR, subject: IRI, predicate: IRI): Statement[] {
  // not required to show to explain ACP
  return [new Statement()];
}

function replacePredicate(statements: Statement[], findPredicate: IRI, replacePredicate: IRI): Statement[] {
  // not required to show to explain ACP
  return [new Statement()];
}
```
