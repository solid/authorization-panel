HTTP Privilege Request Protocol
===============================

Introduction
------------
This memo specifies a protocol between an HTTP client and server for the
client to request additional access privilege when denied access to a resource
by the server for insufficient privilege.

The client will typically use some form of authentication when communicating
with the server. The method and semantics of authentication are beyond the
scope of this memo. Methods could include, but are not limited to, use of the
HTTP `Authorization` header using a scheme supported by the server such as
`Bearer`, or a TLS client certificate.

A client may comprise a software application component that is identified in
some way to the server. The method by which a software application is identified
to the server is beyond the scope of this memo. Methods could include, but
are not limited to, use of the HTTP `Origin` header, or an application identity
associated in some way with the `Bearer` token.

The method by which a server determines whether an HTTP client has sufficient
privilege to access the requested resource is beyond the scope of this memo.
Methods could include, but are not limited to, determining whether the
authenticated identity is a member of a configured role group, or whether the
user associated with the authenticated identity has endorsed the identified
software application currently being used for the access attempted.

The method by which a server determines whether it's appropriate to allow the
client to request additional privilege is beyond the scope of this memo.
Methods could include, but are not limited to, determining whether the
authenticated identity is the controller of the resource, or is the owner of
the server.

The key words "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**", "**SHALL
NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**", "**NOT RECOMMENDED**",
"**MAY**", and "**OPTIONAL**" in this document are to be interpreted as
described in BCP 14 \[[RFC2119][]\] \[[RFC8174][]\] when, and only when, they
appear in all capitals, as shown here.

Operation
---------
The following is a typical exchange according to this protocol.

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
web-based permission management interface, and in some circumstances it might
be appropriate to allow the client's user to access this interface. The
privilege request endpoint **MAY** include in its response a URI [template][RFC6570]
to access this interface, to which the client can redirect the user's trusted
web browser. The interface URI template **SHOULD** include a `redirect_uri`
variable so that the interface can eventually redirect back to the client
application on completion of the management activity.

On completion of the management activity (for example, but not limited to,
in a permission management interface if provided, or offline by whatever means
supported by the server) the client can attempt access again. There is no
guarantee that sufficient privilege has been granted at this point, and new
requests might still be denied, potentially restarting this process.

Syntax
------
The link to the privilege request endpoint is in an HTTP `Link` response
header of the original denied request. The link relation is *TBD*, but for
now let us use `x-permission-request` as a placeholder pending an agreeable
identifier. The link target URI is opaque to the client, need not be for the
same origin as the original denied request, **SHOULD** be unguessable, and
**SHOULD** only be valid for a limited time.  The privilege request endpoint
**MUST** be able to sufficiently identify the client solely by this URI, for
example by the URI comprising a key to a database record, or by comprising a
signed serialization of relevant information about the client.  The `Link`
header **SHOULD** include an `expires_in` parameter giving the number of
seconds after the `Date` of this response at which the privilege request URI
will no longer be valid.

A request for additional privilege **SHALL** be made by HTTP `POST` to the
privilege request URI. The endpoint **MUST NOT** require any special credentials,
including any authentication or authorization credentials included with the
original request -- that is, the URI itself is a sufficient [capability][]
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

* *Key TBD* (**OPTIONAL**): a URI [template][RFC6570] string for a web-based
  permission management interface. For now let us use `x-permission-management-page`
  as a placeholder key pending an agreeable identifier.  If followed by the
  client, the URI **SHOULD** be opened in the user's trusted web browser, not
  in a frame. The web-based permission management interface **SHOULD** use
  frame-busting techniques and **SHOULD** take care to defeat cross-site
  request forgery, click-jacking, and similar attacks.  The URI can include
  the following template variable:

  * `redirect_uri` (**RECOMMENDED**): A target to resume the client application.
    The `redirect_uri` can include state information, for example in a fragment
    identifier, to aid resumption of the client.

  Unrecognized template variables **SHOULD** be left blank.

Unrecognized response object keys **MUST** be ignored.  A possible future
extension could include an indication that the completion webhook was recognized
and accepted.

An unsuccessful HTTP response (with code not in `2XX`) indicates the request
was not accepted.

Example Interaction
-------------------
This is a concrete, annotated example interaction according to this protocol.
*N.B.* the `x-permission-*` identifiers are placeholders pending agreeable names.

The client attempts an authenticated access to a resource:

    →
    GET /some/restricted/resource HTTP/2
    Host: alice.example
    Origin: https://other.example
    Authorization: Bearer gZDES1DqHf1i3zydSqfnsgGhkMgc4gcbpnCHSCcQ

The server determines the client has insufficient privilege (for example, the
app the user is using isn't allowed). The server supports requesting additional
privilege and determines it is appropriate to allow the client to do so (for
example, because the authenticated user is the owner).

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
this server provides a web-based interface for managing requests for additional
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
    
The client application determines to present the web-based server management
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


  [RFC2119]:          https://tools.ietf.org/html/rfc2119
  [RFC6570]:          https://tools.ietf.org/html/rfc6570
  [RFC8174]:          https://tools.ietf.org/html/rfc8174
  [capability]:       https://en.wikipedia.org/wiki/Capability-based_security
  [webhook]:          https://en.wikipedia.org/wiki/Webhook
