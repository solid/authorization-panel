# Access Inheritance Use Cases


## Read access on a collection of resources

See also: https://solid.github.io/authorization-panel/authorization-ucr/#inheritance-readonly

Alice can read all resources in a collection.

### ACP

```turtle
ex:Collection1
  acp:accessControl ex:AccessControl1 ;
  ldp:contains ex:Resource1 .

ex:AccessControl1
  acp:applyMembers ex:Policy1 .

ex:Policy1
  acp:anyOf ex:AgentMatcher1 ;
  acp:allow acl:Read .

ex:AgentMatcher1
  acp:agent ex:Alice .
```

Note: Resources linked to a collection via `ldp:contains` will inherit policies linked to the collection's access control via `acp:applyMembers`. Collections linked to a collection via `ldp:contains` will pass the inherited policies through to their member resources.

### WAC

```turtle
ex:Collection1
  ldp:contains ex:Resource1 .

ex:AccessControl1
    acl:agent ex:Alice ;
    acl:default ex:Collection1 ;
    acl:mode acl:Read .
```

Note: A resource will use its parent container's access control `acl:default` authorization unless it has its own access control defined (parentage being defined via the `ldp:contains` predicate). Inheritance is not granular and can be cut off by adding an access control to a resource at any point in the resource hierarchy tree. If the parent container doesn't have an access control, the WAC inheritance algorithm works its way up through `ldp:contains` predicates.
