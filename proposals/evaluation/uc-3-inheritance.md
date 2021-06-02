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

The Weekly status collection is an `ldp:BasicContainer`, which contains a number of `ldp:BasicContainers`, one for each weekly meeting. The advantage of having these as containers rather than plain resources is that any number of other documents can be added to the container too.

```turtle
<.> a ldp:BasicContainer;
  ldp:contains <2021-04-28/>, <2021-05-05/>, <2021-05-12/> .
```  


The `<weekly-status/>` container links to an `acl:accessControl` resource located at `<.acl>` in the same container. So we have the following hierarchy of resources:

```
</work/weekly-status/>
</work/weekly-status/.acl>
</work/weekly-status/2021-04-28/>
</work/weekly-status/2021-05-05/>
</work/weekly-status/2021-05-12/>
```

This acl contains

```turtle
[]  acl:agentClass </groups/research#g1> ;
    acl:default <.> ;
    acl:mode acl:Read .
```

Note: A resource will use its parent container's access control `acl:default` authorization unless it has its own access control defined (parentage being defined via the `ldp:contains` predicate). Inheritance is not granular and can be cut off by adding an access control to a resource at any point in the resource hierarchy tree. If the parent container doesn't have an access control, the WAC inheritance algorithm works its way up through `ldp:contains` predicates.

The downside of this is that adding an access control resource, requires all the relevant rules to be copied to the new acr, creating duplication of rules.