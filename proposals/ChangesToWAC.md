DRAFT PROPOSAL
==============

Consolidated Proposals for Changes to Web Access Control
========================================================
This memo proposes several independent changes to [Web Access Control (WAC)][WAC],
including changes to syntax and form as well as semantics. They are presented
together here by the author for convenience. This memo should not be construed
to represent all current proposed changes to WAC, either by the author or by
others.

Unless specified otherwise, each proposed change is independent of the others,
and may be adopted or declined without affecting the other proposed changes.

The changes enumerated in this memo describe the author's solutions to several
pain points and desired functionalities encountered while independently
implementing an access control server.

Terminology
-----------
The key words "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**", "**SHALL
NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**", "**NOT RECOMMENDED**",
"**MAY**", and "**OPTIONAL**" in this document are to be interpreted as
described in BCP 14 \[[RFC2119][]\] \[[RFC8174][]\] when, and only when, they
appear in all capitals, as shown here.

Web Access Control Recommended, Not Required
============================================
In consideration that WAC was designed for the social access models of Solid,
and to promote multiple implementations of specialty access management
applications from which an ordinary user could choose, [WAC][] **SHOULD** be
implemented by a *Solid-compliant* server that is intended as generic storage
for ordinary users' data.  However, because the implementation, usage,
deployment, and security considerations and circumstances of servers aren't
universal, and WAC is not a universal solution to the access control problem,
WAC is therefore not required-to-implement for a server to be *Solid-compliant*.

The HTTP `Link` header link relation `rel="acl"`, if present, **MUST** refer
to an access control resource conforming to WAC.

Discussion
----------
It is anticipated that ordinary applications will not need (nor, in most
situations would they be able) to read or modify access control resources.
Server implementations should be free to experiment with new access control
systems and regimes (such as, without limitation, capability-based access
control, role-based access control, or attribute-based access control) and
new vocabularies, semantics, or representations of access control rules.

It is anticipated that modifications to access control rules will be made by
specialty (and specially privileged) permission management applications. So
that the user can have maximum choice among possible such applications, it
is **RECOMMENDED** that at least the user's storage implement WAC. However,
the user is free to choose *any* storage that provides the normal Solid REST
interface and semantics, including ones that do not implement WAC.

Ordinarily, the user (or the user via the app she is using) will not be a
controller of a resource, and in this circumstance it doesn't matter what the
representation or semantics of any access control mechanism is for the resource.


<a name="effective-origin"></a>All Requests Have an Effective Origin/App
========================================================================
An HTTP request for a resource, for purposes of WAC, **SHALL** be construed
to have an effective origin even if an `Origin` header or app identifier is
not explicitly present in (or semantically associated to) the syntax elements
of the request. If the request does not have an application identifier (such
as one derived directly or indirectly from the contents of an `Authorization`
header) or an `Origin` header, the request **SHALL** be considered to have
an effective origin that is the same as the origin of the requested URI. In
other words, if not otherwise specified, the default origin for a request is
the origin of the requested URI.

Discussion
----------
The purpose of this requirement is twofold: to allow the existence of
`acl:Authorization`s that do not allow an app in the same origin as the
requested resource; and to conform to the semantics of the `Origin` header,
which considers a request with no `Origin` header and one with the same origin
as the requested URI to be "[same origin][]".

Implication
-----------
This change requires that all `acl:Authorization`s specify at least one app
or origin (or indirectly with a [tag][TagProposal]) in order to authorize a
request (though an app that is authorized by the user for tag `"*"` for the
resource's origin will match an `acl:Authorization` that specifies no tags).


Same `Authorization` for Apps and Agents
========================================
In order that different apps can be authorized for different users (or groups
or classes of users), an `acl:Authorization` **MUST** authorize both the agent
associated with the request as well as the app or effective origin associated
with the request. This replaces the current definition in WAC that, when an
app is being used (that is, when a request has an effective origin), that
there must be an `acl:Authorization` for the app/origin as well, but it doesn't
need to be the same one that authorizes the user.

Discussion
----------
It's anticipated that in most cases, for any particular access mode, there
will be one `acl:Authorization` specifying the users and apps/origins/tags
allowed, and this requirement will not be burdensome.

This change is to enable specifying, for example, that User A can only use
app `https://app1.example`, and that User B can only use app `https://app2.example`.
Currently in WAC there is no way to represent this case, since if
`https://app2.example` was allowed, then User A would be able to use that
app.


Match the Origin of `acl:origin`'s Object
=========================================
In order to portably specify the origin of the requested URI, when evaluating
whether an `acl:origin` in an `acl:Authorization` matches the effective origin
of a request, the match **SHALL** be made against the origin of the `acl:origin`'s
object, rather than against the triple's object exactly.

Discussion
----------
This allows one to specify an `acl:origin` with a relative URI in an access
control resource (particularly in Turtle), since the relative URI mechanism
provides no way to derive an origin (that is, a URI with an empty `path-abempty`
part). For example:

    [] a acl:Authorization;
       acl:origin </>;
       ...

or even

    [] a acl:Authorization;
       acl:origin <>;
       ...

This will be particularly useful if
[All Requests Have an Effective Origin/App](#effective-origin).


Wildcard `"*"` for `acl:origin`
===============================
A resource controller might wish to allow an access from an authorized
user/agent using any application. An `acl:origin` being the special string
literal `"*"` **SHALL** match any effective request origin (including the
anonymous/unknown origin and the [same origin][]). This may be particularly
useful when granting access to all users (`foaf:Agent`, which includes
unauthenticated ones) and all origins, if
[All Requests Have an Effective Origin/App](#effective-origin).


Replace `acl:accessTo` with `acl:accessToClass`
===============================================
Currently in WAC, `acl:accessTo` specifies the resource to which an
`acl:Authorization` applies. In sensical access control resources, this will
always be the URI of the single resource whose access control resource this
is, except in the special (and confusing) case of an access control resource
for a container, where the `acl:Authorization` is intended to be considered
only for inherited resources but not for the container whose access control
resource this is. This usage also reduces the portability of the access control
resource, by requiring to modify it if its associated resource is moved.

This section proposes to obsolete `acl:accessTo` and its confusing semantics,
and replace them with a portable `acl:accessToClass` and an enumerated list of
special classes to use with it:

  * `acl:Resource` - the class of all resources subject to WAC (the default if no
    class is specified);
  * `acl:Container` - the class of all containers/directories;
  * `acl:Document` - the class of all non-container resources;
  * `acl:SubResource` - the class of all resources excluding the container
    whose Access Control Resource this is;
  * `acl:SubContainer` - the class of all containers excluding the one whose
    Access Control Resource this is.

An `acl:accessToClass acl:Resource` **SHALL** be inferred as the default if
no `acl:accessToClass` is explicitly specified in an `acl:Authorization`.

Discussion
----------
This proposal removes specific URIs to which an `acl:Authorization` applies
from the Authorization, so that the access control resource becomes portable
(that is, it and the resource to which it applies can be moved without having
to modify the access control resource). The resource classes are both explicit
(and therefore easier to understand) and more general, allowing different
treatment for collections and documents from a single inherited access control
document.

The default of `acl:accessToClass acl:Resource` allows for more concise access
control rules in the common case.


Change `acl:default` Range to Boolean
=====================================
Considering that in practice an access control resource is associated with
one ordinary resource, and to promote portability of access control resources,
the range of `acl:default` is `xsd:boolean`. If not specified, the default
value is `false`.

A value of `false` (including if implicit) means that this `acl:Authorization`
**SHALL NOT** be eligible to authorize an access if this access control
resource is being considered because it is inherited by a subresource. Any
non-`false` value is considered boolean-`true`, and means that this
`acl:Authorization` can be considered when this access control resource is
inherited by a subresource.

Discussion
----------
For well-formed and sensical `acl:Authorization`s in current WAC, an `acl:default`
will either not be present (indicating the `Authorization` is not eligible
for consideration when inherited) or it will be the URI of the resource whose
access control resource this is. In practice it will never be any other URI.
In consideration of this, the proposed change is backward-compatible with
existing access control resources.

By changing the range of `acl:default` to `xsd:boolean`, this predicate doesn't
refer to a specific resource URI, and so its containing document (along with
the resource to which it applies) can be moved easily without needing to
modify the object of the triple.


The `acl:Search` Access Mode
============================
This section proposes a new permission mode `acl:Search` for
containers/folders/directories. This mode can be understood to be substantially
equivalent to the "execute" permission for POSIX/Unix directories. This mode
allows traversal of a container/folder/directory to a resource. `acl:Search`
**MUST** be granted (explicitly or implicitly) from the permission root (the
base path prefix subject to WAC) down through containers to the leaf resource,
including the leaf itself if it is a container.

In an access control resource for a container having no `acl:Authorization`
containing an `acl:mode acl:Search`, the gatekeeper **SHALL** allow `acl:Search`
to all, including inheritors, as if the following were present:

    []
        a                 acl:Authorization;
        acl:mode          acl:Search;
        acl:agentClass    foaf:Agent;
        acl:origin        "*";
        acl:accessToClass acl:Container;
        acl:default       true .

This default behavior allows a resource controller to omit specifying
`acl:Search` in an access control resource unless specific control over this
mode is desired. This can simplify the access control resource in the normal
case, and is backward-compatible with existing access control resources.

When using [tags][TagProposal], an authorization by the user for a tag for
the app for *any* access mode **SHALL** authorize that tag for the app for
`acl:Search`.


Excluding Agents and Origins from `acl:Authorizations`
======================================================
In social networking, it is common to exclude a user, or group of users, from
access to a resource (for example, to ban a disruptive user). In another case,
it might be desirable to exclude a member of an existing group from access
to a resource without having to define a new group (for example, to invite
all of one's friends to a surprise birthday party for one friend, but to
exclude the birthday friend so it remains a surprise).

Two new predicates for an `acl:Authorization` are proposed for these cases:

  * `acl:excludeAgent`: **NO** `acl:Authorization`, where the Agent is the
    object of this triple, **SHALL** be eligible to authorize an Access;

  * `acl:excludeAgentGroup`: **NO** `acl:Authorization`, where the Agent is
    a member of the group that is the object of this triple, **SHALL** be
    eligible to authorize an Access.

In other words, if the accessing user is excluded by `acl:excludeAgent` or
`acl:excludeAgentGroup`, then the `acl:Authorization` excluding the user will
not be considered when determining if an access is allowed. Note that being
excluded from one `acl:Authorization` doesn't preclude another `acl:Authorization`
in the same access control resource from granting access to the user.

Combining `acl:excludeAgent` or `acl:excludeAgentGroup` with mode `acl:Search`
can provide an easy way to ban one or a group of users from an entire resource
hierarchy (where resources might have distinct access control resources) by
only needing to change the one access control resource at the top of the
hierarchy.

Likewise, it may be desirable to make a best effort to not allow access to a
resource to a user when using an app from a specific origin (for example, if
the origin is known to be malicious):

  * `acl:excludeOrigin`: **NO** `acl:Authorization`, where the effective
    origin of the request matches the origin of the object of this triple,
    **SHALL** be eligible to authorize an Access.


Controlling App Privilege With `acl:app` and `acl:tag`
======================================================
The predicates `acl:app` and `acl:tag`, and their usage and semantics, are
[specified in detail in a separate proposal][TagProposal].



  [TagProposal]: ReplaceTrustedAppsWithTags.md
  [WAC]:         https://github.com/solid/web-access-control-spec
  [RFC2119]:     https://tools.ietf.org/html/rfc2119
  [RFC8174]:     https://tools.ietf.org/html/rfc8174
  [same origin]: https://tools.ietf.org/html/rfc6454#section-3
