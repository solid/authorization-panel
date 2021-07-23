# Access Inheritance Use Cases

This is part 3 of the [implementation specific use cases comparison](./use-cases.md).

See also: [UCR 2.3 Collection resource inherited access](https://solid.github.io/authorization-panel/authorization-ucr/#uc-inheritance)


## Setup

The Weekly status collection is an `ldp:BasicContainer`, which contains other `ldp:BasicContainers`, one for each weekly meeting, into which further documents can be placed. 

```turtle
# Resource: </weekly-status/>
<.>
  a ldp:BasicContainer ;
  ldp:contains <2021-04-28/>, <2021-05-05/>, <2021-05-12/> .
```

We have the following hierarchy of resources (shown in more detail in the UCR):

```
</weekly-status/>
</weekly-status/2021-04-28/>
</weekly-status/2021-04-28/report.md>
</weekly-status/2021-05-05/>
</weekly-status/2021-05-05/report.md>
</weekly-status/2021-05-05/diagram.jpg>
</weekly-status/2021-05-12/>
```

We omit here the access-control resources as those are proposal specific.

## 1. Read access to a group on a collection of resources

We want to enable read access to all resources contained in `</weekly-status/>` for a group of people (`ex:Alice` & `ex:Bob`).

### ACP

Bob and Alice are part of the agent matcher `</acp/research#m1>`:

```turtle
# Resource: </acp/research>
<#m1>
  a acp:AgentMatcher ;
  acp:agent ex:Bob, ex:Alice .
```

`</acp/research>` is visible only to members of the research group. 

The policy `</acp/research#p1>` gives read access to all agents matched by `</acp/research#m1>`:

```turtle
# Resource: </acp/research>
<#p1>
  a acp:Policy ;
  acp:anyOf <#m1> ;
  acp:allow acl:Read .
```

The access control `</weekly-status/.acp#ac1>` applies to all resources contained in `</weekly-status/>` and enables read access to all agents matched by `</acp/research#m1>` via policy `</acp/research#p1>`:

```turtle
# Resource: </weekly-status/.acp>
<#ac1>
  a acp:AccessControl ;
  acp:apply </acp/research#p1> ; # applies the policy to the container resource </weekly-status/>
  acp:applyMembers </acp/research#p1> . # applies the policy to all resources contained by </weekly-status/>
```

#### a. Accessing `</weekly-status/>`

An unauthenticated agent making a GET on the `</weekly-status/>` container will receive a `Link: <.acp>; rel="acl"` header in the `401` response that points to the above `<.acp>`. 
This relation makes the contents of `</weekly-status/.acp>` authoritative and is, therefore, the information the client can use to decide which credentials to present.

[Actors](https://essif-lab.pages.grnet.gr/framework/docs/terms/actor) whose [principal](https://essif-lab.pages.grnet.gr/framework/docs/essifLab-glossary#principal) are not members of the research institute, will not have access to the policy `</acp/research#p1>` and so, not knowing what credential to present will stop there.

#### b. Accessing `</weekly-status/2021-04-28/report.md>`

Any [digital actor](https://essif-lab.pages.grnet.gr/framework/docs/terms/digital-actor) making a request to  `</weekly-status/2021-04-28/report.md>` will receive in the header of a 401 response, a link to `</weekly-status/2021-04-28/report.acp>`. 
If that actor makes a `GET` request to `<report.acp>` it will receive a graph isomorphic to:

```turtle
# Resource: </weekly-status/2021-04-28/report.acp>
<#ac1>
  a acp:AccessControl ;
  acp:apply </acp/research#p1> .
```

Note that the triples in `</weekly-status/2021-04-28/report.acp>` are generated from the statements in `</weekly-status/.acp>`. 
All resources in ACP have their own associated effective Access Control Resource.

The same logic thus applies here as for access to the container. To know which credentials to present, a digital actor will need to know the contents of `</acp/research>`. But access to that is only granted to members of that research organization.
   
### WAC

Bob and Alice are members of the research group `</groups/research#g1>`:

```turtle
# Resource: </groups/research>
<#g1>
  a vcard:Group ;
  vcard:hasMember ex:Bob ;
  vcard:hasMember ex:Alice .
```

This vcard is visible only to members of the research group. 

#### a. Accessing `</weekly-status/>`

The acl enabling read access to all resources contained in `</weekly-status/>` for all members of group `</groups/research#g1>` is:

```turtle
# Resource: </weekly-status/.acl>
<#authorization>
  a acl:Authorization ;
  acl:agentGroup </groups/research#g1> ;
  acl:default <.> ;
  acl:mode acl:Read .
```

Any agent making a GET on the `</weekly-status/>` container will receive a `Link: <.acp>; rel="acl"` header in the `401` response that points to the above `<.acl>`. 
This relation makes the contents of `</weekly-status/.acl>` authoritative and is, therefore, the information the client must use to decide what credentials to present, and the resource's Guard to decide whether to accept those credentials.

At present, though, only the controller of `</weekly-status/.acl>` can read that resource, as WAC does not have an option to make the ACL more widely readable. (Some proposals: adding a [ControlRead](https://github.com/solid/web-access-control-spec/issues/85) mode or [ACLs on ACLs](https://github.com/solid/authorization-panel/issues/189)).

#### b. Accessing `</weekly-status/2021-04-28/report.md>`

A unauthenticated client actor making a GET request to  `</weekly-status/2021-04-28/report.md>` may receive in the header of a `401`, either of the following: 

1) `Link: </weekly-status/.acl>; rel=acl`. 
2) `Link: </weekly-status/2021-04-28/report.acl>; rel=acl`

As pointed out previously, only the controller and the resource guard can read those; all other actors (e.g., Alice or Bob) need undefined out-of-band knowledge to know how to authenticate. We are therefore limited to the controller actor.

In (1), the actor whose Principal is the Controller will be able to determine the credentials it can use from the `acl:default` rules. 

In (2), either the `acl` resource has already been created, in which case the default rule of `</weekly-status/.acl>` does not apply, or the `acl` resource has not yet been created, so the default applies, but the client cannot know what the default `acl` is.

### WAC+ acls on acls or extension to modes

With [issue 189: ACLs on ACLs](https://github.com/solid/authorization-panel/issues/189) it becomes possible to make the ACLs more widely readable than the controller, which means that the clients of Bob and Alice can find out that they have access to the resource.

Another option is to extend the modes with a ControlRead mode [as mentioned in issue 85](https://github.com/solid/web-access-control-spec/issues/85). 

## 2. Changing permissions to a subcollection

Bob wants to give Carol read/write access to the "2021-04-28" weekly status, and nothing more; that is, to the collection `</weekly-status/2021-04-28/>` and every resource it contains, but not to any other resource in the collection `</weekly-status/>`.

(What example from the UCR does this correspond to best?)

### ACP 

Carol is part of the agent matcher `</acp/research#m2>`:

```turtle
# Resource: </acp/research>
<#m2>
  a acp:AgentMatcher ;
  acp:agent ex:Carol .
```

The research policy `</acp/research#p2>` gives read access to all agents matched by `</acp/research#m2>`:

```turtle
# Resource: </acp/research>
<#p2>
  a acp:Policy ;
  acp:anyOf <#m2> ;
  acp:allow acl:Read, acl:Write .
```

The access control resource `</weekly-status/2021-04-28/.acp#ac1>` initially starts off as containing relevant triples taken from the relevant `applyMembers` rules of `</weekly-status/.acp>`.

To allow Carol to have access to `</weekly-status/2021-04-28/>` and children, the Controller of `2021-04-28/` needs to edit the rules in `</weekly-status/2021-04-28/.acp>` to something like:

```turtle
# Resource: </weekly-status/2021-04-28/.acp>
<#ac1>
  a acp:AccessControl ;
  acp:apply </acp/research#p1>, </acp/research#p2> ;
  acp:applyMembers </acp/research#p1>, </acp/research#p2> . # applies the policy to all resources contained by </weekly-status/2021-04-28/>
```


### WAC

To give Carol read and write access to the `</weekly-status/2021-04-28/>` collection and its content, Bob (todo: is bob the controller?) must create a new effective ACL resource, `</weekly-status/2021-04-28/.acl>` which would express all permissions that are to govern access to `</weekly-status/2021-04-28/>`.

Todo: How does a client create a new acl if one does not exist before? This is not so easy, as the 
`</weekly-status/2021-04-28/>` can contain one of the following headers
1. `Link: </weekly-status/.acl>; rel="acl"`
2. `Link: </weekly-status/2021-04-28/.acl>; rel="acl"`
3. no Link header

In case 2 the controller agent will know what to edit. But not in case 1 or 3. But since all 3 are left open by the WAC spec, the client cannot know without out-of-band knowledge which resource to edit.

Given that out of band knowledge, the actor wanting to give access permissions to Carol to `</weekly-status/.acl>` resource, will need to copy the relevant rules from `</weekly-status/.acl>` to the newly created `</weekly-status/2021-04-28/.acl>` and include Carol too. 

```Turtle
# Resource: </weekly-status/2021-04-28/.acl>
<#authorization>
  a acl:Authorization ;
  acl:agentGroup </groups/research#g1> ;
  acl:default <.> ;
  acl:mode acl:Read .

<#new-authorization>
  a acl:Authorization ;
  acl:agent ex:Carol ;
  acl:default <.> ;
  acl:mode acl:Read, acl:Write .
```

As soon as this ACL is created, the old ACL will no longer be authoritative. 

The problem here is that we need to copy many of the statements from `</weekly-status/.acl>` into the new acl; this means that subsequent changes to the root acl will not get automatically propagated, and the more resources there are, the more places will need to be edited to carry through any future changes made to the root acl.

### WAC+ relaxing acl:default

If one were to want the `</weekly-status/2021-04-28/>` ACR to continue to use `</weekly-status/.acl>` then one could make the `acl:default` more flexible as follows.

According to the [ACL ontology definition as of July 2021](https://github.com/solid/authorization-panel/pull/216#discussion_r665338497), the `acl:default` predicate is only effective in statements where the current container is the object; that is, the resource `</weekly-status/.acl>`, which is the direct effective access control list of `</weekly-status/>`, can only target that directory in statements using `acl:default`.

However, if WAC's use of `acl:default` were to be relaxed as described in [issue 191](https://github.com/solid/authorization-panel/issues/191), then one could rely on the effective access control resource discovery mechanism and augment the content of `</weekly-status/.acl>`:

```Turtle
# Resource: </weekly-status/.acl>
<#authorization>
  a acl:Authorization ;
  acl:agentGroup </groups/research#g1> ;
  acl:default <.> ;
  acl:mode acl:Read .

<#new-authorization>
  a acl:Authorization ;
  acl:agent ex:Carol ;
  acl:default <./2021-04-28/> ;
  acl:mode acl:Read, acl:Write .
```

See also: https://github.com/solid/authorization-panel/pull/216#discussion_r665230245

As a result the ACLs for all the resources could be pointing to the root ACL. But doing that will tend to place all the ACLs on a Pod together, giving anyone with access to that ACL, visibility on all the access control rules of the Pod.

### WAC+ ac:imports +acr 

[WAC+:imports](https://github.com/solid/authorization-panel/issues/210) is explained most easily if we also require every resource to link to its own ACR, as ACP does.
This makes it easy for a client to find out where the effective ACR is.

We keep `wac:default` working as currently specified. Note, that this proposal is also compatible with more flexible acl:defaults, and indeed with the use of general `wac:accessToClass` descriptions, such as classes of resources with a specific tag.
 
An unauthenticated client that makes a `GET` on `</weekly-status/2021-04-28/>` will receives a `401` with a `Link` to `</weekly-status/2021-04-28/.acl>`. 
The  container when newly created would contain the following triple:

```Turtle
# Resource: </weekly-status/2021-04-28/.ac>

<> ac:imports <../.acl> .
```

This allows any client (including the controller's client) to find the default rule by following their nose.

The current WAC inheritance algorithm states that the automatic inheritance no longer has effect for a resource that has an ACL, but that does not stop inheritance from from being explicitly defined using `ac:imports`.

The Actor for the Principal in control of the container can then  `PUT` the following rules to allow Carol to read and write to those containers:

```Turtle
# Resource: </weekly-status/2021-04-28/.ac>
<>
  ac:imports <../.acl> .

<#new-authorization>
  a acl:Authorization ;
  acl:agent ex:Carol ;
  acl:default <.> ;
  acl:mode acl:Read, acl:Write .
```

This removes the need to duplicate the `</weekly-status/.acl#authorization>` rule:  any future edits to that rule need only be done in one place.
