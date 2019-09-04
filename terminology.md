# Terminology

## User

Person identified with [WebID](https://www.w3.org/2005/Incubator/webid/spec/identity/).
Assumed as trusted unless explicitly called **malicious user**.

## WebID Profile

Document resulting from dereferencing WebID [as specified](https://www.w3.org/2005/Incubator/webid/spec/identity/).
No assumptions made about where it stays hosted.

## Resource Server (RS) [RFC6749]

The server hosting the protected resources, capable of accepting and responding to protected resource requests using access tokens.

## Solid detail

Server hosting resources protected with [Web Access Control](https://github.com/solid/web-access-control-spec).

## Resource Owner [RFC6749]

An entity capable of granting access to a protected resource. When the resource owner is a person, it is referred to as an end-user.

### Solid detail

User with [`acl:Control`](https://github.com/solid/web-access-control-spec#aclcontrol) access mode.
No assumptions on relationship to Resource Server hosting it.

## Authorization Server (AS) [RFC6749]

The server issuing access tokens to the client after successfully authenticating the resource owner and obtaining authorization.

## OpenID Provider (OP) [OpenID.Core]

OAuth 2.0 Authorization Server that is capable of Authenticating the End-User and providing Claims to a Relying Party about the Authentication event and the End-User.

### Solid detail [WebID-OIDC]

WebID Profile can link to it using [`solid:oidcIssuer`](https://github.com/solid/webid-oidc-spec#issuer-discovery-from-webid-profile) predicate.

## Storage

LDP container hosted on Resource Server,
WebID Profile can link to it using [`space:storage`](https://github.com/solid/solid-spec/blob/master/solid-webid-profiles.md#storage-discovery). Resource Server can host multiple storages.

## Client [RFC6749]

An application making protected resource requests on behalf of the resource owner and with its authorization.  The term "client" does not imply any particular implementation characteristics (e.g., whether the application executes on a server, a desktop, or other devices).

## Client Software [RFC7591]

Software implementing an OAuth 2.0 client.

## Client Instance [RFC7591]

A deployed instance of a piece of client software.

## References
* [RFC6749](https://tools.ietf.org/html/rfc6749) The OAuth 2.0 Authorization Framework
* [RFC7591](https://tools.ietf.org/html/rfc7591) OAuth 2.0 Dynamic Client Registration Protocol
* [OpenID.Core](https://openid.net/specs/openid-connect-core-1_0.html#Terminology) OpenID Connect Core 1.0
* [WebID-OIDC](https://github.com/solid/webid-oidc-spec) WebID-OIDC Authentication

------------

TODO: reconcile with https://github.com/solid/webid-oidc-spec#decentralized-authentication-glossary
