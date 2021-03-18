# Evaluation of proposals for Authorization

* ✅ Requirement is fulfilled
* ❌ Requirement is not fulfilled
* 🔸 Status of requirement fulfillment to be determined

| WAC | WAC+ | ACP | Requirement | 
| --- | --- | --- | --- |
| 🔸 [w1.1]| 🔸 | 🔸  | [3.1.1. The system shall allow access to be limited based on the identity of the agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-agent-identity) |
| 🔸 | 🔸 [p1.2] | 🔸 | [3.1.2. The system shall allow access to be limited based on the identity of the agent, only when that identity is issued by a trusted identity provider.](https://solid.github.io/authorization-panel/authorization-ucr/#req-trusted-identity) |
| 🔸 | 🔸 | 🔸 | [3.1.3. The system shall allow access to be limited to an agent based on the agent’s membership in a certain group of agents.](https://solid.github.io/authorization-panel/authorization-ucr/#req-agent-group) |
| 🔸 | 🔸 | 🔸 | [3.1.4. The system shall allow access to be limited to an agent based on the client application in use by the agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-application) |
| 🔸 | 🔸 | 🔸 | [3.1.5. The system shall allow an agent to limit modes and/or conditions of access for a given client application in their use for a resource or collection that they have been granted access to.](https://solid.github.io/authorization-panel/authorization-ucr/#req-client-constrained) |
| 🔸 | 🔸 | 🔸 | [3.1.6. The system shall allow access to be permitted for any unauthenticated or authenticated agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-public) |
| 🔸 | 🔸 | 🔸 | [3.1.7. The system shall allow access to be limited to any authenticated agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-authenticated) |
| 🔸 | 🔸 | 🔸 | [3.2.1. The system shall allow access to be limited to an agent based on the agent’s possession of a certain verifiable credential or capability.](https://solid.github.io/authorization-panel/authorization-ucr/#req-vc) |
| 🔸 | 🔸 [p2.2]  | 🔸 | [3.2.2. The system shall ensure that there are practical and efficient mechanism available for the client to determine an appropriate credential to present for access to a given resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-vc-determine) | 
| 🔸 | 🔸 | 🔸 | [3.3.1. The system shall allow the ability to read the access permissions associated with a certain resource to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-read-permissions) |
| 🔸 | 🔸 | 🔸 | [3.3.2. The system shall allow the ability to change the access permissions associated with a certain resource to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-change-permissions) |
| 🔸 | 🔸 | 🔸 | [3.3.3. The system shall provide the effective access permissions on a certain resource or collection as they relate to the agent making the request, in the request response.](https://solid.github.io/authorization-panel/authorization-ucr/#req-effective-modes) |
| 🔸 | 🔸 | 🔸 | [3.3.4. The system shall allow the ability to read a certain resource to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-read) |
| 🔸 | 🔸 | 🔸 | [3.3.5. The system shall allow the ability to change any of the existing contents of a certain resource to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-write) |
| 🔸 | 🔸 | 🔸 | [3.3.6. The system shall allow the ability to change existing data in a certain resource to be limited, such that only new data can be added to it.](https://solid.github.io/authorization-panel/authorization-ucr/#req-append) |
| 🔸 | 🔸 | 🔸 | [3.3.7. The system shall limit the ability to delete a certain resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-delete) |
| 🔸 | 🔸 | 🔸 | [3.3.8. The system shall allow for access to a resource or collection to be limited to the agent that created it.](https://solid.github.io/authorization-panel/authorization-ucr/#req-creator) |
| 🔸 | 🔸 | 🔸 | [3.3.9. The system shall not rely on the URI path to identity resources or collections](https://solid.github.io/authorization-panel/authorization-ucr/#req-uripath) |
| 🔸 | 🔸 | 🔸 | [3.4.1. The system shall allow the ability to read a certain collection to be limited, exposing only the data from the collection resource itself, and a listing of its members, and excluding the contents of its members, or any metadata about them.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-read) |
| 🔸 | 🔸 | 🔸 | [3.4.2. The system shall allow the ability to change data specific to a certain collection to be limited, including only the data from the collection resource itself, and excluding any additions or subtractions from its list of members.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-write) |
| 🔸 | 🔸 | 🔸 | [3.4.3. The system shall allow the ability to create a resource in a certain collection to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-create) |
| 🔸 | 🔸 | 🔸 | [3.4.4. The system shall limit the ability to delete a resource in a certain collection.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-delete) |
| 🔸 | 🔸 | 🔸 | [3.4.5. The system shall allow for the creator of a resource in a certain collection to be automatically granted access to the created resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-creator) |
| 🔸 | 🔸 | 🔸 | [3.4.6. The system shall allow the ability to read the access permissions associated with a certain collection to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-read-permissions) |
| 🔸 | 🔸 | 🔸 | [3.4.7. The system shall allow the ability to change the access permissions directly associated with a certain collection to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-change-permissions) |
| 🔸 | 🔸 | 🔸 | [3.5.1. The system shall allow for a certain collection to specify access permissions that are inherited by its members.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-inheritance) |
| 🔸 | 🔸 | 🔸 | [3.5.2. The system shall allow for a certain resource to be read if the agent has inherited read access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-readonly) |
| 🔸 | 🔸 | 🔸 | [3.5.3. The system shall allow for a resource to be changed if the agent has inherited write access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-change) |
| 🔸 | 🔸 | 🔸 | [3.5.4. The system shall allow for new data to be added to a resource, without being able to change existing data in that resource, if the agent has inherited append access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-appendonly) |
| 🔸 | 🔸 | 🔸 | [3.5.5. The system shall allow for new resources to be added to a given collection if the agent has inherited create access from the parent collection that the given collection is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-create) |
| 🔸 | 🔸 | 🔸 | [3.5.6. The system shall allow for resources to be deleted from a given collection if the agent has inherited delete access from the parent collection that the given collection is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-delete) |
| 🔸 | 🔸 | 🔸 | [3.5.7. The system shall allow for the members of a certain collection to extend or augment the permissions inherited from the parent collection.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-modify) |
| 🔸 | 🔸 | 🔸 | [3.5.8. The system shall allow for a certain collection to specify access permissions that are inherited by its members and cannot be augmented.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-force) |
| 🔸 | 🔸 | 🔸 | [3.5.9. The system shall allow for the default permissions of a newly created resource to be inherited from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-default-permissions) |
| 🔸 | 🔸 | 🔸 | [3.5.10. The system shall allow for the access permissions directly associated with a certain resource to be read if the agent has inherited read permission access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-read-permissions) |
| 🔸 | 🔸 | 🔸 | [3.5.11. The system shall allow for the access permissions directly associated with a certain resource to be changed if the agent has inherited write permission access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-change-permissions) |
| 🔸 | 🔸 | 🔸 | [3.6.1. The system shall allow the ability to limit access to a certain resource by a given start and/or end data and time.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-time) |
| 🔸 | 🔸 | 🔸 | [3.6.2. The system shall allow the ability to limit access to a certain resource by a tag associated with that resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-tag) |
| 🔸 | 🔸 | 🔸 | [3.6.3. The system shall allow the ability to limit access to a certain resource based on the existence of a specific relationship with another resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-relationship) |
| 🔸 | 🔸 | 🔸 | [3.6.4. The system shall allow access to be limited to only a subset of data in a certain resource based on supplied filter criteria.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-filter) |
| 🔸 | 🔸 | 🔸 | [3.6.5. The system shall allow the access modes and/or conditions of a given access permission for a certain resource or collection to change after other specified conditions have been satisfied.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-action) |
| 🔸 | 🔸 | 🔸 | [3.6.6. The system shall allow the ability to read, create, or change only those access permissions for a given resource or collection that apply to a specified group of agents to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-control) |
| 🔸 | 🔸 | 🔸 | [3.6.7. The system shall allow the ability to read, create, or change access permissions for resources associated with a particular tag to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-control-tag) |

## Notes

### [3.1.1. The system shall allow access to be limited based on the identity of the agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-agent-identity)

#### 3.1.1 WAC
[w1.1]:#311-wac

https://github.com/solid/authorization-panel/issues/176

### [3.1.2. The system shall allow access to be limited based on the identity of the agent, only when that identity is issued by a trusted identity provider.](https://solid.github.io/authorization-panel/authorization-ucr/#req-trusted-identity)

#### 3.1.2 WAC+
[p1.2]:#312-wac

https://github.com/solid/authorization-panel/issues/176


### [3.1.3. The system shall allow access to be limited to an agent based on the agent’s membership in a certain group of agents.](https://solid.github.io/authorization-panel/authorization-ucr/#req-agent-group)


### [3.1.4. The system shall allow access to be limited to an agent based on the client application in use by the agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-application)

### [3.1.5. The system shall allow an agent to limit modes and/or conditions of access for a given client application in their use for a resource or collection that they have been granted access to.](https://solid.github.io/authorization-panel/authorization-ucr/#req-client-constrained)

### [3.1.6. The system shall allow access to be permitted for any unauthenticated or authenticated agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-public) 

### [3.1.7. The system shall allow access to be limited to any authenticated agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-authenticated)

### [3.2.1. The system shall allow access to be limited to an agent based on the agent’s possession of a certain verifiable credential or capability.](https://solid.github.io/authorization-panel/authorization-ucr/#req-vc)

### [3.2.2. The system shall ensure that there are practical and efficient mechanism available for the client to determine an appropriate credential to present for access to a given resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-vc-determine)

#### 3.2.2 WAC+
[p2.2]:#322-wac

[issue 189: ACLs on ACLs for WAC](https://github.com/solid/authorization-panel/issues/189)

### [3.3.1. The system shall allow the ability to read the access permissions associated with a certain resource to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-read-permissions) 

### [3.3.2. The system shall allow the ability to change the access permissions associated with a certain resource to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-change-permissions)

### [3.3.3. The system shall provide the effective access permissions on a certain resource or collection as they relate to the agent making the request, in the request response.](https://solid.github.io/authorization-panel/authorization-ucr/#req-effective-modes)

### [3.3.4. The system shall allow the ability to read a certain resource to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-read)

### [3.3.5. The system shall allow the ability to change any of the existing contents of a certain resource to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-write)

### [3.3.6. The system shall allow the ability to change existing data in a certain resource to be limited, such that only new data can be added to it.](https://solid.github.io/authorization-panel/authorization-ucr/#req-append)

### [3.3.7. The system shall limit the ability to delete a certain resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-delete)

### [3.3.8. The system shall allow for access to a resource or collection to be limited to the agent that created it.](https://solid.github.io/authorization-panel/authorization-ucr/#req-creator)

### [3.3.9. The system shall not rely on the URI path to identity resources or collections](https://solid.github.io/authorization-panel/authorization-ucr/#req-uripath)

### [3.4.1. The system shall allow the ability to read a certain collection to be limited, exposing only the data from the collection resource itself, and a listing of its members, and excluding the contents of its members, or any metadata about them.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-read)

### [3.4.2. The system shall allow the ability to change data specific to a certain collection to be limited, including only the data from the collection resource itself, and excluding any additions or subtractions from its list of members.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-write)

### [3.4.3. The system shall allow the ability to create a resource in a certain collection to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-create)

### [3.4.4. The system shall limit the ability to delete a resource in a certain collection.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-delete)

### [3.4.5. The system shall allow for the creator of a resource in a certain collection to be automatically granted access to the created resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-creator)

### [3.4.6. The system shall allow the ability to read the access permissions associated with a certain collection to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-read-permissions)

### [3.4.7. The system shall allow the ability to change the access permissions directly associated with a certain collection to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-change-permissions)

### [3.5.1. The system shall allow for a certain collection to specify access permissions that are inherited by its members.](https://solid.github.io/authorization-panel/authorization-ucr/#req-collection-inheritance)

### [3.5.2. The system shall allow for a certain resource to be read if the agent has inherited read access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-readonly)

### [3.5.3. The system shall allow for a resource to be changed if the agent has inherited write access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-change)

### [3.5.4. The system shall allow for new data to be added to a resource, without being able to change existing data in that resource, if the agent has inherited append access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-appendonly)

### [3.5.5. The system shall allow for new resources to be added to a given collection if the agent has inherited create access from the parent collection that the given collection is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-create)

### [3.5.6. The system shall allow for resources to be deleted from a given collection if the agent has inherited delete access from the parent collection that the given collection is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-delete)

### [3.5.7. The system shall allow for the members of a certain collection to extend or augment the permissions inherited from the parent collection.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-modify)

### [3.5.8. The system shall allow for a certain collection to specify access permissions that are inherited by its members and cannot be augmented.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-force)

### [3.5.9. The system shall allow for the default permissions of a newly created resource to be inherited from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-default-permissions)

### [3.5.10. The system shall allow for the access permissions directly associated with a certain resource to be read if the agent has inherited read permission access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-read-permissions)

### [3.5.11. The system shall allow for the access permissions directly associated with a certain resource to be changed if the agent has inherited write permission access from the parent collection the resource is a member of.](https://solid.github.io/authorization-panel/authorization-ucr/#req-inheritance-change-permissions)

### [3.6.1. The system shall allow the ability to limit access to a certain resource by a given start and/or end data and time.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-time)

### [3.6.2. The system shall allow the ability to limit access to a certain resource by a tag associated with that resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-tag)

### [3.6.3. The system shall allow the ability to limit access to a certain resource based on the existence of a specific relationship with another resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-relationship)

### [3.6.4. The system shall allow access to be limited to only a subset of data in a certain resource based on supplied filter criteria.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-filter)

### [3.6.5. The system shall allow the access modes and/or conditions of a given access permission for a certain resource or collection to change after other specified conditions have been satisfied.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-action)

### [3.6.6. The system shall allow the ability to read, create, or change only those access permissions for a given resource or collection that apply to a specified group of agents to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-control)

### [3.6.7. The system shall allow the ability to read, create, or change access permissions for resources associated with a particular tag to be limited.](https://solid.github.io/authorization-panel/authorization-ucr/#req-conditional-control-tag)
