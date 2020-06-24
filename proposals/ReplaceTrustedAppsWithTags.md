DRAFT PROPOSAL
==============

Scope Tags in WAC for User Control of App Privilege
===================================================
This memo proposes to replace the "trusted apps" mechanism, and to augment
the `acl:origin` mechanism in [Web Access Control (WAC)][WAC], in order to
enable resource owners and users to control the access privileges that their
various apps have to different classes of resources, by introducing the
notions of

  1. resource owners classifying resources with access scope **tags**; and
  2. users authorizing their apps for accesses to different classes of resources.

Summary
-------
The resource owner classifies her resources by assigning one or more tags to
`acl:Authorizations` in a resource's Web Access Control Resource (ACL). The
owner can choose the same or different tags for different access modes.

The user authorizes sets of tags, and the access modes to which they apply,
for each of the apps she uses. The authorizations can be made per resource
origin or to apply to any resource origin.

The app includes the user's applicable tag authorizations when accessing the
resource server.  The resource server verifies the authorizations and takes
them into account when deciding whether to grant access to the resource.

Terminology
===========
The key words "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**", "**SHALL
NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**", "**NOT RECOMMENDED**",
"**MAY**", and "**OPTIONAL**" in this document are to be interpreted as
described in BCP 14 \[[RFC2119][]\] \[[RFC8174][]\] when, and only when, they
appear in all capitals, as shown here.

Additions to WAC
================
This memo proposes two additions to WAC:

  * `acl:app`
  * `acl:tag`

`acl:app` is similar to `acl:origin`, but specifies a prefix of an app
identifier instead of an app origin. This allows distinguishing multiple apps
at one origin, if desired. This memo assumes that an app identifier will be
an OAuth2 *redirect_uri*, but this is not required. An `acl:app` matches an
app identifier if the app identifier begins with the `acl:app`, unless the
`acl:app` is a URI containing a fragment identifier, in which case it must
match the app identifier exactly.

The resource owner can specify one or more scope tags for an `acl:Authorization`
instead of an `acl:origin` or `acl:app` by using `acl:tag`:

	# ACL for a container of chat messages, allowing
	# read for all authenticated users who are using
	# an app allowing read for tag <https://vocab.example/tags#Chat>.
	[]
	    a acl:Authorization;
	    acl:mode acl:Read;
	    acl:agentClass acl:AuthenticatedAgent;
	    acl:accessTo <./>;

	    # apps the user has tagged tags:Chat or "*" for acl:Read will be allowed.
	    acl:tag <https://vocab.example/tags#Chat>;

	    acl:default <./> .

If any of the tags that the user has assigned to the app that she's using for
this access mode matches any of the tags in the `acl:Authorization`, then
it's as if there was an `acl:app` or `acl:origin` match. Note that there is
no universal vocabulary of tags/scopes; what tags to assign to authorizations
is entirely at the discretion of a resource owner.

App Authorization Document
==========================
The user associates tags for the combination of an app, resource server origin,
and optionally the security realm (the name of the [protection space][realm];
that is, the `realm` authentication parameter of the `WWW-Authenticate` HTTP
response header) in an App Authorization document. Here is an example App
Authorization document assigning tags `tags:Chat` and `tags:Pictures` to app
`https://app.example/oauth/code` when accessing server `https://mike.example`'s
realm `https://mike.example/auth/` for modes `acl:Read` and `acl:Write`:

	# this is world-readable but has an unguessable URI like
	#     <https://mike.example/wac/app-auth/b6d88441302c07700743b8d793ae2a8a.ttl#it>
	# in a non-listable container.
	
	@prefix acl: <http://www.w3.org/ns/auth/acl#> .
	@prefix tags: <https://vocab.example/tags#> .
	
	<#it>
	    a acl:AppAuthorization;
	    acl:resourceServer [
	        acl:origin <https://mike.example>;
	        acl:realm "https://mike.example/auth/"
	    ];
	    acl:app "https://app.example/oauth/code";
	    acl:tagMode [
	        acl:tag tags:Chat, tags:Pictures;
	        acl:mode acl:Read, acl:Write
	    ] .

An `acl:tag` in an `acl:tagMode` can be the special wildcard `"*"`, which
matches any (including none) tags in an ACL. This allows the user to designate
an app as having privilege to all classes of resources, which might be useful
for administrative and management apps.

	@prefix acl: <http://www.w3.org/ns/auth/acl#> .
	
	<#it>
	    a acl:AppAuthorization;
	    acl:resourceServer [ acl:origin <https://mike.example> ]; # any realm
	    acl:origin <https://coolmanager.example>;
	    acl:tagMode [
	        acl:tag "*";
	        acl:mode acl:Read, acl:Write, acl:Control
	    ] .

To support a default App Authorization, the `acl:resourceServer` can give the
literal `"*"` for `acl:origin`, which matches all servers. However, a server
**MUST** ignore any `acl:tag`s containing a wildcard character (`*` or `?`)
if the server's origin is not in the `acl:resourceServer`. In other words, a
wildcard tag `"*"` can only be used with explicitly specified origins.

	@prefix acl: <http://www.w3.org/ns/auth/acl#> .
	@prefix tags: <https://vocab.example/tags#> .
	
	<#it>
	    a acl:AppAuthorization;
	    acl:resourceServer [ acl:origin "*" ];
	    acl:origin <https://otherapp.example>;
	    acl:tagMode [
	        acl:tag tags:Chat;
	        acl:mode acl:Read
	    ] .

If the `acl:resourceServer` specifies an `acl:realm` then it **MUST** match
the server's realm exactly. Specifying an `acl:realm` is not mandatory, but
may be useful if distinguishing between protection spaces at an origin is
desired.

The URI for an App Authorization Document **MUST** be in (at a sub-path of)
an `acl:appAuthorizations` in the user's profile:

	<#me> acl:appAuthorizations </wac/app-auth/> .

This container/directory **SHOULD** be configured to allow read of App
Authorization documents by anyone and any origin; however, to protect the
user's privacy (specifically, what apps the user uses and what resource servers
the user accesses with those apps) including from other apps the user uses,
listing the container's contents should be restricted to only the user, and
then to only the user's trusted authorization management app.

Note: The user **MAY** have any number of `acl:appAuthorizations` containers.

Associating App Authorizations with Requests
============================================
The method by which an app presents its App Authorization documents is specific
to the authentication protocol being used with the resource server.

For protocols employing [OAuth2 Bearer Tokens][RFC6750] issued by a competent
authorization server, such as the proposed
[WebID HTTP Authorization Protocol][zenomt-auth], the protocol **SHOULD**
support presenting App Authorization documents when acquiring the access
token, so that the associated privileges can be loaded, parsed, and verified
once, and bound to the token. The syntax for associating App Authorization
documents with an access token shall be defined by the authentication protocol.
For example, links to App Authorization documents can be included in an
`app_authorizations` claim in a [proof of possession][RFC7800] token with
[WebID HTTP Authorization Protocol][zenomt-auth].

For all other styles of authentication protocol, such as the proposed DPoP
protocol, the App Authorization documents **SHALL** be in an HTTP `Link`
request header with link relation `http://www.w3.org/ns/auth/acl#appAuthorization`:

	...
	Link: <https://mike.example/wac/app-auth/b6d88441302c07700743b8d793ae2a8a.ttl#it>; rel="http://www.w3.org/ns/auth/acl#appAuthorization"
	...

To support reasonable maintenance of default (origin `"*"`) and origin-specific
app authorizations, and static and dynamic generation of app authorization
documents, authentication protocols **SHOULD** support specifying multiple
App Authorization documents. Authentication protocol implementations **SHOULD**
support specifying at least four documents, to allow for static and dynamic
documents for default and origin-specific authorizations. To avoid a traffic
amplification vulnerability, implementations **SHOULD** limit the number of
distinct resources loaded.


App Authorizations Index
========================
The app discovers its App Authorization Document URIs from an App Authorizations
Index file. The location of the index file is linked from the app preferences.

	# App Authorizations index file for app "https://app.example/oauth/code".
	@prefix acl: <http://www.w3.org/ns/auth/acl#> .

	</wac/app-auth/b6d88441302c07700743b8d793ae2a8a.ttl#it>
	    acl:resourceServer [ acl:origin <https://mike.example>; acl:realm "https://mike.example/auth/" ] .

	</wac/app-auth/4f20846c1179e604048a589583dd6f9c.ttl#it>
	    acl:resourceServer [ acl:origin <https://other.example> ] .

	</wac/app-auth/bce7449445c9eab38da95aaf9143c1d0.ttl#it>
	    acl:resourceServer [ acl:origin "*" ] .

To protect the user's privacy, the index file **SHOULD** be readable only by
the user, and then only by the app that the index is for.

Caching
=======
For both performance and scalability, web resources should be cached when
possible.  However, users expect applications to be responsive to changes in
authorization; for instance when granting a new privilege to an app, users
expect the app to be able to use the new privilege immediately.

When App Authorization documents are presented while acquiring an OAuth2
Bearer Token from a competent Authorization Server, it is expected that the
authorization server will at least revalidate all authentication and authorization
resources, to be responsive to changes in authentication and authorization
properties.  The authorization server can influence the revalidation period
by extending or diminishing the lifetime of the access tokens it issues. To
mitigate an amplification or denial-of-service attack, the authorization
server **SHOULD** provide for a minimum caching period (on the order of
seconds) of all authentication and authorization resources it loads.  To
respond immediately to normal changes in authorization, when App Authorizations
are revalidated on each presentation, an app can simply forget its access
tokens and acquire new ones.

When App Authorization documents are presented in HTTP `Link` headers, the
resource server **SHOULD** cache these authorization resources as normal, but
[**SHOULD CONSIDER**](https://tools.ietf.org/html/rfc6919#section-2) reducing
cache lifetimes to be more responsive to changes. Alternatively, permission
management systems
[**WOULD PROBABLY**](https://tools.ietf.org/html/rfc6919#section-5) use fresh
URIs for updated App Authorization documents when possible; note however that
the user's profile URI can't be changed for this purpose.

Out of Scope
============
The method by which an app requests that the user authorize scopes/tags is
beyond the scope of this memo. Methods could include, but are not limited to,
an enumeration of desired tags in an app manifest file, posting a request to
an inbox, new attributes on requests to an authentication server, and
presentation of a request message to the user for manual editing of static
files.

The method by which apps are authorized for tags and modes is beyond the scope
of this memo. Methods could include, but are not limited to, a privileged app
that manages App Authorization documents and index files (which might read
an inbox or otherwise receive requests from apps), a privileged app that the
user uses to directly adjust the authorizations of apps, an authentication
server that does double-duty as an authorization management system and which
dynamically generates App Authorization documents, and manual editing of
static files by the user.

The method by which the tags that are in use by resources on resource servers
can be discovered by apps or users is beyond the scope of this memo. Methods
could include, but are not limited to, resource servers including tag-related
information in HTTP `403` responses, resource servers posting events to an
inbox, resource servers including human-readable information on a FAQ page,
and well-known convention.


  [WAC]:         https://github.com/solid/web-access-control-spec
  [realm]:       https://tools.ietf.org/html/rfc7235#section-2.2
  [zenomt-auth]: https://github.com/zenomt/webid-auth-protocol
  [RFC2119]:     https://tools.ietf.org/html/rfc2119
  [RFC6750]:     https://tools.ietf.org/html/rfc6750
  [RFC7800]:     https://tools.ietf.org/html/rfc7800
  [RFC8174]:     https://tools.ietf.org/html/rfc8174
