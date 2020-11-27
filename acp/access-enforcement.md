```TypeScript
/**
 * agent    : The Agent attempting to access R
 * resource : The Resource being accessed
 * method   : the HTTP method used by A
 *
 * HTTPMethodToAccessMode is the table the in section 'HTTP Method To Access Mode'
 * 
 * Return: true if access is allowed, otherwise false
**/

isAccessAllowed(agent : Agent, resource : Resource, method : HTTPMethod) {

  let requiredMode = HTTPMethodToAccessMode.get(method)

  if getAccessModes(agent, resource).contains(requiredMode) return true

  return false
}
```

```TypeScript
/**
 * agent    : The Agent attempting to access R
 * resource : The Resource being accessed
 * 
 * Return : An Access object containing the access modes permitted by 
 *          the specified agent on the specified resource.
**/

getAccessModes(agent : Agent, resource : Resource) {
  let accessModes = new Access()
  let acr : ACR = r.getAccessControlResource()

  acr.foreach (ac : AccessControl => {
    accessModes.add(evaluatePolicies (ac.apply, agent, resource))
    accessModes.add(evaluatePolicies (ac.applyProtected, agent, resource))
    accessModes.add(evaluatePolicies (ac.applyLocked, agent, resource))
  })

  return accessModes

  evaluatePolicies (policies : [], agent : Agent, resource : Resource) {
    let allowModes = new Access()
    let denyModes  = new Access()

    policies.foreach (p : Policy => {
      if isPolicySatisfied (p, agent, resource) {
        allowModes.add (p.allow)
        denyModes.add (p.deny)
      }
    })

    return allowModes.subtract(denyModes)
  }

}
```

```TypeScript
/**
 * policy   : The Policy to evaluate to determine whether the specified agent satisfies the policy
 * agent    : The Agent attempting to access the resource
 * resource : The Resource being accessed
 * 
 * Return   : true if the Policy is satisfied, otherwise false
**/

isPolicySatisfied (policy : Policy , agent : Agent, resource : Resource) {

  if (policy.anyOf.some (r : Rule => { isRuleSatisfied(r, agent, resource) })
     && policy.allOf.every (r : Rule => { isRuleSatisfied(r, agent, resource) })
     && !policy.noneOf.some (r : Rule => { isRuleSatisfied(r, agent, resource) })) {

    return true
  }
  return false
}
```

```TypeScript
/**
 * rule     : The Rule to evaluate to determine whether the specified agent satisfies the rule
 * agent    : The Agent attempting to access the resource
 * resource : The Resource being accessed
 * 
 * Return   : true if the Rule is satisfied, otherwise false
**/

isRuleSatisfied (rule : Rule, agent : Agent, resource : Resource) {

  if (  rule.includes(ACP.PUBLIC_AGENT)
     || (rule.includes(ACP.AUTHENTICATED_AGENT) && agent.isAuthenticated()) 
     || rule.includes(agent)
     || (rule.includes(ACP.CreatorAgent) && resource.creator().sameAs(agent))
     || rule.groups().some( g : Group => { g.hasMember(agent) })) {

    return true
  }
  return false
}
```

