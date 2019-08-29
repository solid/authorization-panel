DRAFT PROPOSAL
==============

HTTP Privilege Request Protocol
===============================

Abstract
--------
This memo specifies a protocol between an HTTP client and server for the
client to request additional privilege when denied access to a resource by
the server for insufficient privilege.

Introduction
------------
An HTTP client, for example a software application running in a web browser,
can attempt to access a resource on an HTTP server. The server might restrict
access to the resource according to one or more required access privileges,
for example with an Access Control List.

The client might not possess sufficient privilege according to the server,
so the server might deny the requested access. For example, the client's user
may not have endorsed the software application with the server for the requested
mode of access for a class of resources.

In some cases, the server might determine it's appropriate to allow the client
to request additional privilege, so that a future request to access the
resource might be approved. To continue the above example, the server might
allow users to endorse a software application for one or more modes of access
to one or more classes of resources. The client can choose whether to request
additional privilege, if allowed.

The server might provide a browser-based permission management interface for
processing requests for additional privilege. The server might determine it's
appropriate to allow the client to launch this interface for its user, for
example because the server only allows its owner to process these requests
and the server determined the client's user is the owner. The client can
choose whether to launch this interface, if available.

This memo specifies a protocol that the client can use to request additional
privilege when the server denies access to a resource for insufficient
privilege.

Envisioned Use Cases
--------------------
The protocol described in this memo was designed with at least the following
cases in mind. It's not the intent of this section to limit the protocol to
only these examples.

* A Resource Server (RS) could allow its owner to grant access to different
  classes of resources depending on what application the owner uses.
* An RS could allow any visitor to grant access for herself to different
  classes of resources depending on what application she uses, independent
  of other visitors or the applications they use.
* A browser-based permission management interface might only be available to
  the RS's owner, or only to a resource's controllers, or to all visitors.
  The availablility of the interface might depend on the scope of additional
  privilege required, or RS capability, or configuration.
* An RS could allow a visitor to request access to a resource for which the
  visitor currently has no permission at all.
  - The visitor might be a non-person automatic software agent (bot).
  - Processing the request could involve updating Access Control resources
    or adding the visitor to one or more groups or lists.
  - The request might require approval of the resource's controller(s) or
    an administrator (especially for a bot), or be by a multiphase approval
    process.
  - The request might be approved as a result of the person-visitor accepting
    a Terms of Use, or purchasing a right-to-access, or entering an out-of-band
    verification code, or solving a CAPTCHA, in the browser-based management
    interface.
* For privilege requests that require external approval, the browser-based
  permission management interface might be a status or hold screen that could
  return the user to the application immediately on the request being completed
  by an administrator.
* An RS might not allow, or even support, a request for additional privilege.
* A RS might not provide a browser-based management interface at all. Permission
  requests could be sent to an administrator via email or chat, or entered
  into an issue or ticket tracking system, or another kind of work queue.
  Administrators might manage permissions by using command-line tools or
  editing configuration files by hand.
* Any request for additional privilege could be denied or ignored.

Scope
-----
The client will typically use some form of authentication when communicating
with the server. The method and semantics of authentication are beyond the
scope of this memo. Methods could include, but are not limited to, use of the
HTTP `Authorization` header using a scheme supported by the server such as
`Bearer`, or a TLS client certificate, to establish an identity of the client's
user.

A client may comprise a software application component that is identified in
some way to the server. The method by which a software application is identified
to the server is beyond the scope of this memo. Methods could include, but
are not limited to, use of the HTTP `Origin` header, or an application identity
associated in some way with the `Bearer` token.

The method by which a server determines whether an HTTP client has sufficient
privilege to access the requested resource is beyond the scope of this memo.
Methods could include, but are not limited to, determining whether the
authenticated identity is a member of a configured role or group, or whether
the user associated with the authenticated identity has endorsed the identified
software application currently being used for the access attempted.

The method by which a server determines whether it's appropriate to allow the
client to request additional privilege is beyond the scope of this memo.
Methods could include, but are not limited to, determining whether the
authenticated identity is the controller of the resource, or is the owner of
the server.

Terminology
-----------
The key words "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**", "**SHALL
NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**", "**NOT RECOMMENDED**",
"**MAY**", and "**OPTIONAL**" in this document are to be interpreted as
described in BCP 14 \[[RFC2119][]\] \[[RFC8174][]\] when, and only when, they
appear in all capitals, as shown here.

Operation
---------
The following is a typical exchange according to this protocol:

1. The client attempts access to a resource.
2. The server determines the client has insufficient privilege and denies the
   request, returning an HTTP `403 Forbidden` response. The server further
   determines it's appropriate to allow the client to request additional
   privilege, so includes in the response a `Link` for the privilege request
   endpoint URI.
3. The client determines to request additional privilege and makes a simple
   `POST` to the provided privilege request endpoint URI without credentials.
4. The privilege request endpoint determines the request is acceptable and
   queues the request, returning an HTTP `202 Accepted` response.

The privilege management system might (but is not required to) provide a
browser-based permission management interface, and in some circumstances it
might be appropriate to allow the client's user to access this interface. The
privilege request endpoint **MAY** include in its response a [Level 1 URI
template][RFC6570] to access this interface, to which the client can redirect
the user's trusted web browser. The interface URI template **SHOULD** include
a `redirect_uri` variable so that the interface can eventually redirect back
to the client application on completion of the management activity.

On completion of the management activity (for example, but not limited to,
in a permission management interface if provided, or offline by whatever means
supported by the server) the client can attempt access again. There is no
guarantee that sufficient privilege has been granted at this point, and new
requests might still be denied, potentially restarting this process.

Syntax
------
Note: When supporting browser-based applications, servers **SHOULD** take
care to include necessary [Cross-Origin Resource Sharing (CORS)][CORS] response
headers.

The link to the privilege request endpoint is in an [HTTP `Link`][RFC8288]
response header of the original denied transaction. The link relation is
*TBD*, but for now let us use `x-permission-request` as a placeholder pending
an agreeable identifier. The link target URI is opaque to the client, need
not be for the same origin as the original denied request, **SHOULD** be
unguessable and unforgeable, and **SHOULD** only be valid for a limited time
and a single use.  The privilege request endpoint **MUST** be able to
sufficiently identify the client solely by this URI, for example by the URI
comprising a key to a database record, or by comprising a signed serialization
of relevant information about the client.  This link **SHOULD** include an
`expires_in` [target attribute][RFC8288§2.2] giving the number of seconds
after the `Date` of this response at which the privilege request URI will no
longer be valid.

A request for additional privilege **SHALL** be made by HTTP `POST` to the
privilege request URI. The endpoint **MUST NOT** require any special credentials,
including any authentication or authorization credentials included with the
original request; that is, the URI itself is a sufficient [capability][]
for this specific request.

The request body of the `POST` to the privilege request endpoint, if present,
**SHALL** have a content-type of `application/x-www-form-urlencoded`. At this
time no parameters are defined. A possible future extension could include a
[webhook][] to be notified on completion of the management activity. The
privilege request endoint **MUST** ignore unrecognized parameters.

A successful response (`2XX`) to a request for additional privilege indicates
that the request was accepted, not necessarily that it was completed or that
sufficient privilege has been or will be granted. A successful response
**SHOULD** use response code `202 Accepted`.

A successful response **MAY** include a response body. If included, the
response body **MUST** be in `application/json` format encoding a JSON
object. The following object key is defined:

* *Key TBD* (**OPTIONAL**): a [Level 1 URI template][RFC6570] string for a browser-based
  permission management interface. For now let us use `x-permission-management-page`
  as a placeholder key pending an agreeable identifier.  If followed by the
  client, the URI **SHOULD** be opened in the user's trusted web browser, and not
  in a frame. The browser-based permission management interface **SHOULD** use
  frame-busting techniques and **SHOULD** take care to defeat cross-site
  request forgery, click-jacking, and similar attacks.  The URI can include
  the following template variable:

  * `redirect_uri` (**RECOMMENDED**): A target to resume the client application.
    The `redirect_uri` can include state information, for example in a fragment
    identifier, to aid resumption of the client.

Unrecognized response object keys **MUST** be ignored.  A possible future
extension could include an indication that the completion webhook was recognized
and accepted.

An unsuccessful HTTP response (with code not in `2XX`) indicates the request
was not accepted.

Example Interaction
-------------------
This is a concrete, annotated example interaction according to this protocol.
*N.B.* the `x-permission-*` identifiers are placeholders pending agreeable names.
[CORS][] headers are shown.

The client attempts an authenticated access to a resource:

    →
    GET /some/restricted/resource HTTP/2
    Host: alice.example
    Origin: https://other.example
    Authorization: Bearer gZDES1DqHf1i3zydSqfnsgGhkMgc4gcbpnCHSCcQ

The server determines the client has insufficient privilege (for example, the
app the user is using isn't allowed). The server supports requesting additional
privilege and determines it is appropriate to allow the client to do so (for
example, because the authenticated user is the owner):

    ←
    HTTP/2 403 Forbidden
    Access-Control-Allow-Origin: https://other.example
    Access-Control-Expose-Headers: Link
    Content-type: text/html; charset=utf-8
    Cache-control: no-cache, no-store
    Date: Fri, 09 Aug 2019 05:09:18 GMT
    Link: </auth/request-permission?r=8374B650-4974-4E14-A7DF-8729041A96D8>; rel="x-permission-request"; expires_in="600"
    
    <html>Your app is not allowed... Yet.</html>
    
The client application determines to request additional privilege and `POST`s
to the `x-permission-request` URI (without credentials):

    →
    POST /auth/request-permission?r=8374B650-4974-4E14-A7DF-8729041A96D8 HTTP/2
    Host: alice.example
    Origin: https://other.example
    Content-type: application/x-www-form-urlencoded

The server accepts and queues the request for additional privilege. Additionally,
this server provides a browser-based interface for managing requests for additional
privilege, and determines it's appropriate to give the client application a
link to the interface (for example, because the authenticated user to which
the permission request [capability][] was given is the owner):

    ←
    HTTP/2 202 Accepted
    Access-Control-Allow-Origin: https://other.example
    Content-type: application/json; charset=utf-8
    Cache-control: no-cache, no-store
    Date: Fri, 09 Aug 2019 05:09:19 GMT
    
    {
        "x-permission-management-page": "https://alice.example/auth/permission-queue?r=07FE5FA4-A3F1-4F83-A6E4-41220A538BDB&redirect_uri={redirect_uri}"
    }
    
The client application determines to present the browser-based server management
interface to the user. The client opens
`https://alice.example/auth/permission-queue?r=07FE5FA4-A3F1-4F83-A6E4-41220A538BDB&redirect_uri=https://other.example/app/page.html%23state%3D3SNflxua6WiOgQmp1YSSg5dPNOpEG0nubd3p3NthHuJb`,
which is the `x-permission-management-page` URI with a *`redirect_uri`*
substituted in the [template][RFC6570], in a new window of the user's trusted
browser. Note that the `redirect_uri` here contains a fragment identifier
with application state to help the application to resume where it left off.

The user interacts with the management UI, perhaps granting necessary additional
privilege for the original request and potential related requests. Eventually
the management activity concludes, and the management interface redirects
back to the app:

    ← HTTP/2 302 Found
    Date: Fri, 09 Aug 2019 05:10:00 GMT
    Location: https://other.example/app/page.html#state=3SNflxua6WiOgQmp1YSSg5dPNOpEG0nubd3p3NthHuJb

The client application can now resume. Sufficient privilege to access desired
resources might not have been granted during the management activity, so the
entire process might play out again.

TODO
====
* `x-permission-request` link relation needs a real URI.
* `x-permission-management-page` response JSON key needs a real name.
* Add more examples illustrating different use cases.


  [CORS]:             https://www.w3.org/TR/cors/
  [RFC2119]:          https://tools.ietf.org/html/rfc2119
  [RFC6570]:          https://tools.ietf.org/html/rfc6570
  [RFC8174]:          https://tools.ietf.org/html/rfc8174
  [RFC8288]:          https://tools.ietf.org/html/rfc8288
  [RFC8288§2.2]:      https://tools.ietf.org/html/rfc8288#section-2.2
  [capability]:       https://en.wikipedia.org/wiki/Capability-based_security
  [webhook]:          https://en.wikipedia.org/wiki/Webhook
