# Resource Access Use Cases


## Change permissions

See also: https://solid.github.io/authorization-panel/authorization-ucr/#basic-change-permissions

Alice can modify access to a resource.

### ACP

```turtle
ex:Resource1
  acp:accessControl ex:AccessControl1 .

ex:AccessControl1
  acp:access ex:Policy1 .

ex:Policy1
  acp:anyOf ex:AgentMatcher1 ;
  acp:allow acl:Read, acl:Write .

ex:AgentMatcher1
  acp:agent ex:Alice .
```

Note: `acp:access` points to policies applied to the access control, which means the permissions applied to an access control are granular; in other words, could be read-only, for example.

### WAC

```turtle
ex:AccessControl1
    acl:agent ex:Alice ;
    acl:accessTo ex:Resource1 ;
    acl:mode acl:Control .
```

Note: `acl:Control` gives full access to the access control resource.


## Read-write access

See also: https://solid.github.io/authorization-panel/authorization-ucr/#basic-write

Bob can read and modify a resource.

### ACP

```turtle
ex:Resource1
  acp:accessControl ex:AccessControl1 .

ex:AccessControl1
  acp:apply ex:Policy1 .

ex:Policy1
  acp:anyOf ex:AgentMatcher1 ;
  acp:allow acl:Read, acl:Write .

ex:AgentMatcher1
  acp:agent ex:Bob .
```

### WAC

```turtle
ex:AccessControl1
    acl:agent ex:Bob ;
    acl:accessTo ex:Resource1 ;
    acl:mode acl:Read, acl:Write .
```


## Group access

See also: https://solid.github.io/authorization-panel/authorization-ucr/#basic-group

A group of people can Read multiple resources.

### ACP

```turtle
ex:Resource1
  acp:accessControl ex:AccessControl1 .

ex:Resource2
  acp:accessControl ex:AccessControl2 .

ex:AccessControl1
  acp:apply ex:Policy1 .

ex:AccessControl2
  acp:apply ex:Policy1 .

ex:Policy1
  acp:anyOf ex:AgentMatcher1 ;
  acp:allow acl:Read .

ex:AgentMatcher1
  acp:agent ex:Alice, ex:Bob .
```

Note: Here the group of people: Alice & Bob; is defined in `ex:AgentMatcher1` which can be reused. In this specific case, since we want to allow read access to the group on both resources, `ex:Policy1` is being reused.

## WAC

```turtle
ex:AccessControl1
  acl:agentGroup ex:Group1 ;
  acl:accessTo ex:Resource1 ;
  acl:mode acl:Read .

ex:AccessControl2
  acl:agentGroup ex:Group1 ;
  acl:accessTo ex:Resource2 ;
  acl:mode acl:Read .

ex:Group1
  vcard:hasMember ex:Alice, ex:Bob .
```

Note: Here, the access mode needs to be specified for every group reuse.
