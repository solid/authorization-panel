# Effective Access Control Resource Discovery Use Cases

This is part 0 of the [implementation specific use cases comparison](./use-cases.md).

The "effective access control resource" (ACR) of a resource R, is the resource that contains the rules that will be used by the server to determine access to R.


## 1. Discovering a resource's effective access control

A Solid client that wants to decide which credentials to present to access a resource or determine if it can edit the ACR, needs to discover the effective ACR of the resource in question.

### Setup

We have the following hierarchy of resources:

```turtle  
</default.acr>
</foo/bar/baz/x>
</foo/bar/baz/x.acr>
```

### Universal effective ACR discovery

Both WAC and ACP follow an `acl` link header in the response to a resource `R`, in order to find the Access Control Rules. 
(The name "acl" for the type of such a link [is being discussed](https://github.com/solid/authorization-panel/issues/228).) 


We can start both our examples with the client making a `GET` request on `/foo/bar/baz/x` which returns either of the following responses:

  A. The response is successful, but the client then wants to edit the access control rules:
```HTTP
200 Ok
Link: </foo/bar/baz/x.acr>; rel="acl"
Link: <.>; rev="http://www.w3.org/ns/ldp#contains"
Content-Length: 2042
Content-Type: ...

...
```
  B. The response is unsuccessful, and the client wants to find out how to authenticate to gain access:
```HTTP
401 Unauthorized
Link: </foo/bar/baz/x.acr>; rel="acl"
Link: <.>; rev="http://www.w3.org/ns/ldp#contains"
```

The second `Link`, with relation type `ldp:contains`, is needed for WAC.

### ACP

In ACP, every resource has exactly one effective access control resource directly associated with it, and every access control resource directly governs access over itself and exactly one other resource.

In ACP, access control statements can be spread over several resources; that is, an access control resource can reference other resources.

In ACP, the access control system in place (that is, ACP, as opposed to WAC, for example) is indicated via a `Link` header of `rel="type"` `<http://www.w3.org/ns/solid/acp#AccessControlResource>` in HTTP response to a request for an ACR.

On receiving the `404` with the `Link` header given in our example, the client can make a request on `/foo/bar/baz/x.acr`, if it wants to look at the Access Control Rules. 
Since all resources in ACP have an associated ACR, the resource <`/foo/bar/baz/x.acr>` should return a description of the sets of agents that can have access to the resource. 
This may include links to rules published elsewhere.

### WAC

In WAC, the effective ACR of a resource might be the ACR of a parent container of the resource.

In WAC, ACRs are called ACLs (Access Control Lists).

WAC's [Effective ACL Resource](https://solid.github.io/web-access-control-spec/#effective-acl-resource) discovery is described in [the WAC spec](https://solid.github.io/web-access-control-spec/) as follows:

> ##### Effective ACL Resource Algorithm
> To determine the effective ACL resource of a resource, perform the following steps. Returns string (the URI of an ACL Resource).
>
> 1. Let resource be the resource.
> 2. Let aclResource be the ACL resource of resource.
> 3. If resource has an associated aclResource with a representation, return aclResource.
> 4. Otherwise, repeat the steps using the container resource of resource.

Just as with ACP, the client can follow the `Link: <...acr>; rel="acl"` relation to find out the rules of access. 
But then we have two cases with WAC:

1. the ACR exists and returns the rules 
2. the ACR returns a `404 Not Found`

In (1) everything follows like with ACP above.
In (2) the client then needs to start the recursive process of looking for the effective ACR. 
We will detail (2) next.

1. First the client is lucky enough to be shown the reverse `ldp:contains` relation, so it can do a HEAD on that to find its `ACL`.
```HTTP
HEAD /foo/bar/baz/ HTTP/1.1
```
and with luck the server will respond
```HTTP
200 Ok
Link: </foo/bar/baz/.acr>; rel="acl"
Link: </foo/bar/>; rev="http://www.w3.org/ns/ldp#contains"
```
The client can then continue with
```HTTP
GET /foo/bar/baz/.acr HTTP/1.1
```
to which the server will also return
```HTTP
404 Not Found
```
as the resource does not yet exist.

2. As a result the client will need to look up one level in the hierarchy to search for the effective ACR
```HTTP
HEAD /foo/bar/ HTTP/1.1
```
and with luck the server will respond
```HTTP
200 Ok
Link: </foo/bar/.acr>; rel="acl"
Link: </foo/>; rev="http://www.w3.org/ns/ldp#contains"
```
The client can then continue with
```HTTP
GET /foo/bar/baz/.acr HTTP/1.1
```
to which the server will also return
```HTTP
404 Not Found
```
as the resource does not yet exist.

3. As a result the client will need to look up one level in the hierarchy to search for the effective ACR
```HTTP
HEAD /foo/ HTTP/1.1
```
— and with luck the server will respond —
```HTTP
200 Ok
Link: </foo/.acr>; rel="acl"
Link: </>; rev="http://www.w3.org/ns/ldp#contains"
```
The client can then continue with
```HTTP
GET /foo/.acr HTTP/1.1
```
to which the server will also return
```HTTP
404 Not Found
```
as the resource does not yet exist.

4. As a result the client will need to look up one level in the hierarchy to search for the effective ACR
```HTTP
HEAD / HTTP/1.1
```
and with luck the server will respond

```HTTP
200 Ok
Link: </.acr>; rel="acl"
```
The client can then continue with
```HTTP
GET /foo/.acr HTTP/1.1
```
to which the server will finally return the content.
                  
### WAC+NTrig

A resource can let a client know that it supports dataset serialisation its ACR by returning the following header in either the 200 or 401:

```HTTP
Link: </foo/bar/baz/x.acr>; rel="acl"; type="application/trig"
```
                                                         
A client could then follow up with a request to `</foo/bar/baz/x.acr>` with `Accept: application/trig`, which could respond:

```HTTP
200 Ok
Content-Type: application/trig
Content-Length: ...

GRAPH <> {}
GRAPH </.default.acr> {
 <#authorization> a acl:Authorization ;
   acl:agentGroup </owner#me> ;
   acl:default </> ;
   acl:mode acl:Control .
}
```

This should be read as giving the triples in the `</.default.acr>` graph, and specifying that no triples exist in the `</foo/bar/baz/x.acr>`.
With a slight adjustment to the WAC spec, this could still count as there being nothing other than the default, which would therefore still be active.

Note: NTrig has already been proposed in [issue 210: add :imports relation](https://github.com/solid/authorization-panel/issues/210).




## See also

- [Access Control Resource discovery](https://github.com/solid/authorization-panel/issues/228)
