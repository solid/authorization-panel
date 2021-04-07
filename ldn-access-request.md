# Requesting Access with Linked Data Notifications

Status: draft


## Abstract
This proposal describes an exchange of Linked Data Notifications with a specific shape
for the purpose of requesting access to a resource.


## Actors
- **Resource server:**
  server on which the resource is located
- **Requesting agent:**
  agent requesting access to the resource
- **Processing agent:**
  agent accepting or rejecting the access request


## Request flow

1. The requesting agent assumes it does not have access to a given resource `R`.
   For instance, it might have received a `403` in response to `GET` request,
   or a `404` for a resource it knows exists.

2. The requesting agent discovers an _access request inbox_ of `R`,
   of from an agent that has access to `R`.
   This might have been communicated out-of-band,
   or can be linked with a `tdb:accessRequestInbox` relation,
   or with a generic `ldp:inbox` relation.

3. The requesting agent sends an _access request_
   conforming to the shape described below
   using the [Linked Data Notifications](https://www.w3.org/TR/ldn/) Protocol.

4. The processing agent _optionally_ (depending on the request)
   sends an _accept_ or _reject_ notification to an inbox of the requesting agent.

5. The requesting agent retries accessing the resource.


## Request access shape

_This example needs to be formalized into a SHACL and/or ShEx shape._

In this example,
an agent with WebID `https://user/#me`
is requesting Read and Write permissions for itself
on a resource `https://resources/documents/x`.

It submits this message to an inbox of `https://resources/documents/x`
(or to an inbox of an administrators for this resource)
in which it requests appending a permission block
to the resource's ACL document at `https://resources/documents/x.acl`.


```turtle
@prefix acl: <http://www.w3.org/ns/auth/acl#>.
@prefix as: <https://www.w3.org/ns/activitystreams#>.
@prefix tbd: <http://example.org/to-be-determined#>.

[
  a tbd:AppendRequest;
    as:actor <https://user/#me>;
    as:target <https://resources/documents/x.acl>;
    as:object [
      a acl:Authorization;
      acl:agent <https://user/#me>;
      acl:accessTo <https://resources/documents/x>;
      acl:mode acl:Read, acl:Write;
    ];
].
```

When successful, this HTTP request
will result in the creation of a new resource,
the location of which is communicated to the agent
in the `Location` header (as per the LDN/LDP specs).

_Note: the request is currently phrased
as a generic “append” request to a resource,
so it is reusable for other cases as well.
We could also write it as a more specific `tbd:AccessRequest`._


## Access notification shape

_This example needs to be formalized into a SHACL and/or ShEx shape._

Assuming the URL of the created request to be
`https://resources/inbox/1234.ttl`,
the agent can be informed of acceptance or rejection
with a message to its inbox as follows.

```turtle
@prefix as: <https://www.w3.org/ns/activitystreams#>.

[
  a as:Accept; # or as:Reject
    as:object <https://resources/inbox/1234.ttl>;
    as:target  <https://resources/documents/x.acl>; # or just /x
].
```
