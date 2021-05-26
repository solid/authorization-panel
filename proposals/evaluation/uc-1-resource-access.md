# Resource Access Use Cases


## Change permissions

See also: https://solid.github.io/authorization-panel/authorization-ucr/#basic-change-permissions

Alice can modify access to a resume resource.

### ACP

```turtle
ex:ResumeResource1
  acp:accessControl ex:AccessControl1 .

ex:AccessControl1
  acp:access ex:Policy1 .

ex:Policy1
  acp:anyOf ex:AgentMatcher1 ;
  acp:allow acl:Read, acl:Write .

ex:AgentMatcher1
  acp:agent ex:Alice .
```

### WAC

```turtle
ex:AccessControl1
    acl:agent ex:Alice ;
    acl:accessTo ex:ResumeResource1 ;
    acl:mode acl:Control .
```


## Read-write access

See also: https://solid.github.io/authorization-panel/authorization-ucr/#basic-write

Bob can read and modify a resume resource.

### ACP

```turtle
ex:ResumeResource1
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
    acl:accessTo ex:ResumeResource1 ;
    acl:mode acl:Read, acl:Write .
```
