```TypeScript
/**
 * Evaluate all the acp:Policy statements in each acp:AccessControl
 * in the ACR for the Resource.
 * Accumulate all of the acp:AccessModes for the acp:allow and
 * acp:deny statements for each acp:Policy that is satisfied.
 * Subtracting the accumulated deny modes from the accumulated allow
 * modes provides the access modes permitted.
 *
 * The access mode required is determined by the request method used.
 * The table in section 'HTTP Method To Access Mode' provides the
 * mapping from request method to access mode.
 *
 * agent    : The Agent attempting to access the Resource
 * resource : The Resource being accessed
 * method   : the HTTP method used by the Agent
 *
 * HTTPMethodToAccessMode is the table the in section 'HTTP Method To Access Mode'
 *
 * Return: true if access is allowed, otherwise false
 **/

export function isAccessAllowed(agent: Agent, resource: Resource, method: HTTPMethod): boolean {
  const acr: ACR = resource.getAccessControlResource();
  const accessModes: AccessModes = new AccessModes();

  acr.getAccessControls().forEach((ac: AccessControl) => {
    accessModes.add(evaluatePolicies(ac.apply, agent, resource));
    accessModes.add(evaluatePolicies(ac.applyProtected, agent, resource));
    accessModes.add(evaluatePolicies(ac.applyLocked, agent, resource));
  });

  const requiredMode: string = HTTPMethodToAccessMode.get(method);
  if (accessModes.contains(requiredMode)) return true;

  return false;
}

/**
 * Evaluate all the acp:Policy statements using acp:access
 * in the ACR for the Resource.
 * Accumulate all of the acp:AccessModes for the acp:allow and
 * acp:deny statements for each acp:Policy that is satisfied.
 * Subtracting the accumulated deny modes from the accumulated allow
 * modes provides the access modes permitted.
 *
 * The access mode required is determined by the request method used.
 * The table in section 'HTTP Method To ACR Access Mode' provides the
 * mapping from request method to access mode.
 *
 * agent    : The Agent attempting to access the ACR
 * resource : The Resource associated with the ACR being accessed
 * method   : the HTTP method used by the Agent
 *
 * Return: true if access is allowed, otherwise false
 */
export function isAcrAccessAllowed(agent: Agent, resource: Resource, method: HTTPMethod) {
  return isAcrAccessPoliciesAllowed(agent, resource, method, resource.getAccessControlResource().access);
}

export function isAcrAccessMembersAllowed(agent: Agent, resource: Resource, method: HTTPMethod) {
  return isAcrAccessPoliciesAllowed(agent, resource, method, resource.getAccessControlResource().accessMembers);
}

export function isAcrAccessProtectedAllowed(agent: Agent, resource: Resource, method: HTTPMethod) {
  return isAcrAccessPoliciesAllowed(agent, resource, method, resource.getAccessControlResource().accessProtected);
}

export function isAcrAccessMembersProtectedAllowed(agent: Agent, resource: Resource, method: HTTPMethod) {
  return isAcrAccessPoliciesAllowed(
    agent,
    resource,
    method,
    resource.getAccessControlResource().accessMembersProtected
  );
}

export function isAcrAccessLockedAllowed(agent: Agent, resource: Resource, method: HTTPMethod) {
  return isAcrAccessPoliciesAllowed(agent, resource, method, resource.getAccessControlResource().accessLocked);
}

export function isAcrAccessMembersLockedAllowed(agent: Agent, resource: Resource, method: HTTPMethod) {
  return isAcrAccessPoliciesAllowed(agent, resource, method, resource.getAccessControlResource().accessMembersLocked);
}

// Helper functions

/**
 * agent    : The Agent attempting to access the ACR
 * resource : The Resource being accessed
 * method   : The HTTP method used by the Agent
 * policies : The list of Policies to evaluate to determine whether access should be provided
 *
 * HTTPMethodToAcrAccessMode is the table the in section 'HTTP Method To ACR Access Mode'
 *
 * Return: true if access is allowed, otherwise false
 */
function isAcrAccessPoliciesAllowed(agent: Agent, resource: Resource, method: HTTPMethod, policies: Policy[]) {
  const acr: ACR = resource.getAccessControlResource();
  const requiredMode = HTTPMethodToAcrAccessMode.get(method);

  if (evaluatePolicies(policies, agent, acr).contains(requiredMode)) return true;

  return false;
}

/**
 * policies : The list of Policies to be evaluated
 * agent    : The Agent attempting to access the Resource
 * resource : The Resource the Agent is attempting to access
 *
 * Return   : An Access object specifying the access modes permitted on
 *            the Resource by this Agent.
 *            The permitted modes are Read, Write, Append.
 *            If the Resource is an ACR then the Append mode is not used.
 */
function evaluatePolicies(policies: Policy[], agent: Agent, resource: Resource) {
  const allowModes = new AccessModes();
  const denyModes = new AccessModes();

  policies.forEach((p: Policy) => {
    if (isPolicySatisfied(p, agent, resource)) {
      allowModes.add(p.allow);
      denyModes.add(p.deny);
    }
  });

  return allowModes.delete(denyModes);
}

/**
 * policy   : The Policy to evaluate to determine whether the specified agent satisfies the policy
 * agent    : The Agent attempting to access the resource
 * resource : The Resource being accessed
 *
 * Return   : true if the Policy is satisfied, otherwise false
 **/

function isPolicySatisfied(policy: Policy, agent: Agent, resource: Resource) {
  if (
    policy.anyOf.some((r: Rule) => {
      isRuleSatisfied(r, agent, resource);
    }) &&
    policy.allOf.every((r: Rule) => {
      isRuleSatisfied(r, agent, resource);
    }) &&
    !policy.noneOf.some((r: Rule) => {
      isRuleSatisfied(r, agent, resource);
    })
  ) {
    return true;
  }
  return false;
}

/**
 * rule     : The Rule to evaluate to determine whether the specified agent satisfies the rule
 * agent    : The Agent attempting to access the resource
 * resource : The Resource being accessed
 *
 * Return   : true if the Rule is satisfied, otherwise false
 **/

function isRuleSatisfied(rule: Rule, agent: Agent, resource: Resource) {
  if (
    rule.includes(ACP.PUBLIC_AGENT) ||
    (rule.includes(ACP.AUTHENTICATED_AGENT) && agent.isAuthenticated()) ||
    rule.includes(agent) ||
    (rule.includes(ACP.CREATOR_AGENT) && resource.creator().sameAs(agent)) ||
    rule.groups().some((g: Group) => {
      g.hasMember(agent);
    })
  ) {
    return true;
  }
  return false;
}
```
