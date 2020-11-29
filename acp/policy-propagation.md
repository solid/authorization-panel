### Resource


#### PUT Child Resource

When an [Agent](definitions.md#agent) tries to create or update a resource in a Container with the PUT HTTP method, the following algorithm is used to enforce access control and to propagate [Access Policies](definitions.md#access-policy).

```TypeScript
/**
 * agent     : The Agent trying to PUT the resource
 * resource  : The Resource the Agent is trying to PUT
 * container : The parent Container for the Resource
 * content   : The content for the Resource the Agent wants to PUT in the Container
 * 
 * Return    : 403 or 401 if the Agent does not have permission.
 *             204 if the resource already exists and if overwritten successfully.
 *             201 if the resource did not already exists and is created.
 */
putResource (agent : Agnet, resource : Resource, container : Container, content : Stream) {

  if (resource.exists()) {
    if (!isAccessAllowed (resource, agent, HttpMethodsEnum.PUT) return accessError (agent)

    resource.write (content)  
    return StatusCodes.NO_CONTENT
  }

  if (!isAccessAllowed (container, agent, HttpMethodsEnum.PUT) return accessError (agent)

  createResource (container, resource.getIri(), content)

  return StatusCodes.CREATED
}
```

#### POST Child Resource

When an [Agent](definitions.md#agent) tries to create a resource in a Container with the POST HTTP method, the following algorithm is used to enforce access control and to propagate [Access Policies](definitions.md#access-policy).

```TypeScript
/**
 * When an Agent sends a POST request to a container, 
 * a new child Resource will be created in the container. 
 * 
 * agent     : The Agent trying to POST to the Container
 * container : The Container the Agent sent the request to
 * slug      : The hint provided by the requestor for the child resource name
 * content   : The content for the Resource the Agent wants to POST to the Container
 * 
 * Return    : 403 or 401 if the Agent does not have permission.
 *             201 if the resource is created.
 */
postResource (agent : Agent, container : Container, slug : string, content : Stream) {

  if (!isAccessAllowed (container, agent, HttpMethodsEnum.POST) return accessError (agent)

  let iri : string = container.newResourceName (slug)
  createResource (container, iri, content)

  return StatusCodes.CREATED
}
```
#### PATCH Child Resource

When an [Agent](definitions.md#agent) tries to update a resource in a Container with the PATCH HTTP method, the following algorithm is used to enforce access control.

```TypeScript
/**
 * agent    : The Agent tryping to PATH the resource. 
 * resource : The Resource the Agent is trying to PATCH
 * content  : The content the Agent is using to PATCH the Resource. This pseudo-code does 
 *            not attemptto describe how the patch operation are carried out. 
 *
 * Return   : 403 or 401 if the Agent does not have permission.
 *            204 if the resource is patched successfully.
 * 
 */
patchResource (agent : Agent, resource : Resource, content : Stream) {

  if (!isAccessAllowed (resource, agent, HttpMethodsEnum.PATCH) return accessError (agent)
  if (!resource.exists()) return StatusCodes.NOT_FOUND

  applyPatch (content)

  return StatusCodes.NO_CONTENT
}
```

#### DELETE Child Resource

When an [Agent](definitions.md#agent) tries to delete a resource in a Container with the DELETE HTTP method, the following algorithm is used to enforce access control.

```TypeScript
/**
 * Delete a resource and the associated ACR.
 * 
 * NOTE : For discussion. I think it would be better if we had a specific DELETE 
 *        access rather than overloading the WRITE access. This would be on the
 *        resource directly rather than on the parent container. 
 */
deleteResource (agent : Agent, resource : Resource, container : Container) {

  if (!isAccessAllowed (resource, agent, HttpMethodsEnum.DELETE) return accessError (agent)
  if (!resource.exists()) return StatusCodes.NOT_FOUND

  container.delete (resource)
  container.deleteACR (resource)
  container.deleteContainmentTriples (resource)

  return StatusCodes.NO_CONTENT
}
```

### Container

#### PUT Child Container

When an [Agent](definitions.md#agent) tries to create a Container in a parent Container with the PUT HTTP method, the following algorithm is used to enforce access control and to propagate [Access Policies](definitions.md#access-policy).

```TypeScript
/**
 * agent      : The Agent trying to PUT the container
 * container  : The Container the Agent is trying to PUT
 * pContainer : The parent Container for the container 
 * content    : The content for the Container the Agent wants to PUT in the parent Container.
 *              The content cannot be containment triples but can be any triples that are
 *              valid in a normal Resource.
 * 
 * Return     : 403 or 401 if the Agent does not have permission.
 *              204 if the container already exists and the non containment triples have
 *              been overwritten successfully.
 *              201 if the container did not already exist and has been created.
 */
putContainer (agent : Agent, container : Container, pContainer : Container, content : Stream) {
  if (continaer.exists()) {
    if (!isAccessAllowed (container, agent, HttpMethodsEnum.PUT) return accessError (agent)

    // A Container can hold triples just like a normal Resource. 
    // The containment tripes are managed by the server and cannot 
    // be edited directly with a PUT request.
    container.write (content)  

    return StatusCodes.NO_CONTENT
  }

  if (!isAccessAllowed (pContainer, agent, HttpMethodsEnum.PUT) return accessError (agent)

  createContainer (pContainer, container.getIri(), content)

  return StatusCodes.CREATED
}
```

#### POST Child Container

When an [Agent](definitions.md#agent) tries to create a Container in a parent Container with the POST HTTP method, the following algorithm is used to enforce access control and to propagate [Access Policies](definitions.md#access-policy).

```TypeScript
/**
 * agent     : The Agent trying to POST to the Container
 * container : The Container the Agent sent the request to
 * slug      : The hint provided by the requestor for the child container name
 * content   : The content for the Resource the Agent wants to POST to the Container
 * 
 * Return    : 403 or 401 if the Agent does not have permission.
 *             201 if the child container is created.
 */
postContainer (agent : Agent, container : Container, slug : string, content : Stream) {

  if (!isAccessAllowed (container, agent, HttpMethodsEnum.POST) return accessError (agent)

  let iri : string = container.newResourceName (slug)
  createContainer (container, iri, content)

  return StatusCodes.CREATED
}
```

#### PATCH Child Container

When an [Agent](definitions.md#agent) tries to update the content of a Container with the PATCH HTTP method, the following algorithm is used to enforce access control. Note, the containment triples will not be updated but the other Container content will be updated i.e. a container can contain triples other than containment triples.

```TypeScript
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
patchContainer (agent : Agent, container : Container, content : Stream) {

  if (!isAccessAllowed (container, agent, HttpMethodsEnum.PATCH) return accessError (agent)
  if (!container.exists()) return StatusCodes.NOT_FOUND

  // NOTE : Should we ignore containment triples contained in the patch request
  //        or respond with an error?? The current implementation ignores them.
  applyPatch (content)

  return StatusCodes.NO_CONTENT
}
```
#### DELETE Child Container 

When an [Agent](definitions.md#agent) tries to delete a Container in the parent Container with the DELETE HTTP method, the following algorithms are used to enforce access control. 

There are 4 approaches available : 

1. Delete the container only if it is empty
2. Delete the container and its children. If any child cannot be deleted then continue to the next child. Return a list of the children that could not be deleted. This is similar to how most file systems work.
3. Delete the container and its children. All children are deleted or the operation fails. This requires transactional support.
4. Delete the container and its children. If the agent has permission to delete the container, then the permissions for the children are ignored and the children are deleted.

In addition, the delete operation can be carried out synchronously or asynchronously. If the client asks for the operation to be carried out asynchronously and the server is capable of an asynchronous operation then the response will be 202 (ACCEPTED). When the operation is complete the server will send a notification to the parent container along with the result of the operation. This has not been implemented yet so there is no work done on the structure of the notification or result. 

NOTE: We have not done any work on how a client would negotiate with the server on which delete mechanism to use. We could use a mechanism similar to content negotiation. 

##### 1. Delete if empty

```TypeScript
/**
 * Delete a container is it is empty
 * 
 * agent      : The Agent trying to delete the container
 * container  : The Container to delete
 * pContainer : The parent Container of the Container to delete
 * 
 * Return     : 403 or 401 if the Agent does not have permission.
 *              403 if the container is not empty.
 *              204 if the container is successfully deleted.
 */
deleteContainer (agent : Agent, container : Container, pContainer : Container) {

  if (!isAccessAllowed (container, agent, HttpMethodsEnum.DELETE) 
    return accessError (agent)

  if (!container.exists()) return StatusCodes.NOT_FOUND

  if (!container.isEmpty()) return StatusCodes.FORBIDDEN

  pContainer.deleteContainer (container)
  pContainer.deleteACR (container)
  pContainer.deleteContainmentTripes (container)
    
  return StatusCodes.NO_CONTENT
}
```

##### 2. Delete recursively - non atomic

```TypeScript
/**
 * Delete a container and all the children in the container.
 * This is not an atomic function.
 * 
 * agent      : The Agent trying to delete the container
 * container  : The Container to delete
 * pContainer : The parent Container of the Container to delete
 * 
 * Return     : 403 or 401 if the Agent does not have permission.
 *              200 if some of the children could be deleted and a list of 
 *              the IRIs that could not be deleted.
 *              204 if all the children could be deleted.
 */
deleteContainer (agent : Agent, container : Container, pContainer : Container) {

  if (!isAccessAllowed (container, agent, HttpMethodsEnum.DELETE) 
    return (accessError (agent), container.getIri())

  if (!container.exists()) return (StatusCodes.NOT_FOUND, container.getIri())

  let cannotDelete = []

  container.getChildren().foreach (resource : Resource => {
    if (resource.isContainer()) { 
      let response : number
      let fails : string[]
      (response, fails) = deleteContainer (agent, resource, container)
      cannotDelete.concat (fails)
    } else {
      if (deleteResource (agent, resource, container) !== StatusCodes.NO_CONTENT)
        cannotDelete.push (resource.getIri()) 
    }
  })

  if (container.isEmpty()) {
    pContainer.deleteContainer (container)
    pContainer.deleteACR (container)
    pContainer.deleteContainmentTripes (container)
    
    return (StatusCodes.NO_CONTENT, cannotDelete)
  } else {
    cannotDelete.push (container.getIri())

    return (StatusCodes.OK, cannotDelete)
  }
}
```

##### 3. Delete recursively - atomic

```TypeScript
/**
 * Delete a container recursively
 * 
 * agent      : The Agent trying to delete the container
 * container  : The Container to delete
 * pContainer : The parent Container of the Container to delete
 * 
 * Return     : 403 or 401 if the Agent does not have permission.
 *              204 if the container is successfully deleted.
 */
@Transactional
deleteContainer (agent : Agent, container : Container, pContainer : Container) {

  if (!isAccessAllowed (container, agent, HttpMethodsEnum.DELETE) 
    return accessError (agent)

  container.getChildren().foreach (resource : Resource => {
    let result : number = StatusCodes.NO_CONTENT
    if (resource.isContainer()) { 
      if ((result = deleteContainer (agent, resource, container)) !== StatusCode.NO_CONTENT)
        return result
    } else {
      if ((result = deleteResource (agent, resource, container)) !== StatusCodes.NO_CONTENT)
        return result
    }
  })

  pContainer.deleteContainer (container)
  pContainer.deleteACR (container)
  pContainer.deleteContainmentTripes (container)
    
  return StatusCodes.NO_CONTENT
}
```

##### 4. Delete recursively - no recursive permissions

```TypeScript
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
deleteContainer (agent : Agent, container : Container, pContainer : Container) {

  if (!isAccessAllowed (container, agent, HttpMethodsEnum.DELETE) 
    return accessError (agent)

  deleteContainerIgnorePermissions (agent, container, pContainer)

  return StatusCodes.NO_CONTENT

  deleteContainerIgnorePermissions (agent : Agent, container : Container, pContainer : Container) {
    container.getChildren().foreach (resource : Resource => {
      if (resource.isContainer()) deleteContainerIgnorePermissions (agent, resource, container)
      else {
        container.delete (resource)
        container.deleteACR (resource)
        container.deleteContainmentTriples (resource)
      }
    })

    pContainer.deleteContainer (container)
    pContainer.deleteACR (container)
    pContainer.deleteContainmentTripes (container)
  }    
}

#### Helper Functions

```TypeScript
/**
 * This function is called when an agent is attempting to access a resource but
 * does not have permission.
 * 
 * agent : The agent attempting to access the resource
 *
 * Return: 403 if the agent is logged in, 401 if the agent is not logged in.
 */
accessError (agent : Agent) {
  if (agent.isAuthenticated()) return StatusCodes.FORBIDDEN

  return StatusCodes.UNAUTHORIZED 
}
```

```TypeScript
/**
 * Create a new resource in a container. Propagate all the relevant polcies 
 * from the parent container ACR to the new resource ACR.
 *
 * container : The Container in which the resource will be created
 * iri       : The IRI for the new resource. This must be a child of the container 
 * content   : The content to write to the new resource
 */
@Transactional
createResource (container : Container, iri : IRI, content : Stream) {
  let resource : Resource = container.createResource (iri)
  container.createACR (iri) // Create ACR for resource at iri
  container.updateContainmentTriples (resource)
  if (content) resource.write (content)
  propagatePolicies (container, resource)
}
```

```TypeScript
/**
 * Create a new container in an existing container. Propagate all the relevant polcies 
 * from the parent container ACR to the new container ACR.
 *
 * container : The Container in which the new container  will be created
 * iri       : The IRI for the new container. This must be a child of the container 
 * content   : The content to write to the new container. Note this cannot include
 *             containment triples.
 */
@Transactional
createContainer (pContainer : Container, iri : IRI, content : Stream) {
  let container : Container = pContainer.createContainer (iri)
  pContainer.createACR (iri) // Create ACR for container at iri
  pContainer.updateContainmentTriples (container)
  // write the non containment triples
  if (content) container.write (content)
  propagatePoliciesToContainer (pContainer, container)
}
```

```TypeScript
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
propagatePolicies (container : Container, resource : Resource) {
  let acr = resource.getAccessControlResource()
  let cacr = container.getAccessControlResource()

  propagate (cacr, cacr.getIri(), ACP.ACCESS_MEMBERS, acr, ACP.ACCESS)
  propagate (cacr, cacr.getIri(), ACP.ACCESS_MEMBERS_PROTECTED, acr, ACP.ACCESS_PROTECTED)
  propagate (cacr, cacr.getIri(), ACP.ACCESS_MEMBERS_LOCKED, acr, ACP.ACCESS_LOCKED)

  cacr.getAccessControls().foreach (ac : AccessControl => {
    propagate (cacr, ac.getIri(), ACP.APPLY_MEMBERS, acr, ACP.APPLY)
    propagate (cacr, ac.getIri(), ACP.APPLY_MEMBERS_PROTECTED, acr, ACP.APPLY_PROTECTED)
    propagate (cacr, ac.getIri(), ACP.APPLY_MEMBERS_LOCKED, acr, ACP.APPLY_LOCKED)
  }

  return
}
```

```JavaScript
/**
 * Propagate policies from the parent container ACR to the 
 * child container ACR. 
 *
 * Parent Container              Child Container
 * acp:accessMembers          -> acp:access
 * acp:accessMembers          -> acp:accessMembers
 * acp:accessMembersProtected -> acp:accessMembers
 * acp:accessMembersProtected -> acp:accessMembersProtected
 * acp:accessMembersLocked    -> acp:accessLocked
 * acp:accessMembersLocked    -> acp:accessLockedLocked
 * acp:applyMembers           -> acp:apply
 * acp:applyMembers           -> acp:applyMembers
 * acp:applyMembersProtected  -> acp:applyProtected
 * acp:applyMembersProtected  -> acp:applyMembersProtected
 * acp:applyMembersLocked     -> acp:applyLocked
 * acp:applyMembersLocked     -> acp:applyMembersLocked
 */
propagatePoliciesToContainer (pContainer : Container, container : Container) {

  let pAcr = pContainer.getAccessControlResource()
  let cAcr = container.getAccessControlResource()

  propagate (pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS, cAcr, ACP.ACCESS)
  propagate (pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS, cAcr, ACP.ACCESS_MEMBERS)
  propagate (pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS_PROTECTED, cAcr, ACP.ACCESS_PROTECTED)
  propagate (pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS_PROTECTED, cAcr, ACP.ACCESS_MEMBERS_PROTECTED)
  propagate (pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS_LOCKED, cAcr, ACP.ACCESS_LOCKED)
  propagate (pAcr, pAcr.getIri(), ACP.ACCESS_MEMBERS_LOCKED, cAcr, ACP.ACCESS_MEMBERS_LOCKED)

  pAcr.getAccessControls().foreach (ac : AccessControl => {
    propagate (pAcr, ac.getIri(), ACP.APPLY_MEMBERS, cAcr, ACP.APPLY)
    propagate (pAcr, ac.getIri(), ACP.APPLY_MEMBERS, cAcr, ACP.APPLY_MEMBERS)
    propagate (pAcr, ac.getIri(), ACP.APPLY_MEMBERS_PROTECTED, cAcr, ACP.APPLY_PROTECTED)
    propagate (pAcr, ac.getIri(), ACP.APPLY_MEMBERS_PROTECTED, cAcr, ACP.APPLY_MEMBERS_PROTECTED)
    propagate (pAcr, ac.getIri(), ACP.APPLY_MEMBERS_LOCKED, cAcr, ACP.APPLY_LOCKED)
    propagate (pAcr, ac.getIri(), ACP.APPLY_MEMBERS_LOCKED, cAcr, ACP.APPLY_MEMBERS_LOCKED)
  }

  return
}
```

```TypeScript
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
propagate (cacr : ACR, csubject : Subject, cpredicate : Predicate, 
           acr : ACR, rpredicate : Predicate) {

  let statemets : Statment[] = findStatements (cacr, csubject, cpredicate)

  // The base IRI for the subject would also need to be replace if absolute IRIs are used.
  // so that the Resource IRI rather than the Container IRI is used. 
  let resourceStatements : Statement[] = replacePredicate (statements, cpredicate, rpredicate)

  acr.addStatements (resourceStatements)

  return
}
```

