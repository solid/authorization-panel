# Effective Access Control Resource Discovery Use Cases

This is part 0 of the [implementation specific use cases comparison](./use-cases.md).

The resource mandating access over another resource is called its effective access control resource (ACR).

## 1. Discovering a resource's effective access control

A Solid client needs to discover effective ACR in order to understand and or edit access permissions.

Both WAC and ACP use the same effective ACR discovery mechanism.

### Setup

We have the following hierarchy of resources:

```
</x>
</x.acr>
```

### Universal effective ACR discovery

An agent making a GET or HEAD HTTP request on `</x>` will receive a `Link: </x.acr>; rel="acl"` header in the response that points to the above `</x.acr>`.

The Link header with relationship type `acl` indicates the access control resource of a resource.

### ACP

In ACP, every resource has exactly 1 effective access control resource directly associated to it and every access control resource directly mandates access over exactly one resource and itself.

In ACP, access control statements can be spread over several resources, that is, an access control resource can reference other resources.

In ACP, the access control system in place, that is, ACP (as opposed to for example WAC), is indicated via a Link header of `rel="type"` `<http://www.w3.org/ns/solid/acp#AccessControlResource>`.

### WAC

In WAC, the effective ACR of a resource might be the ACR of a parent container of the resource.

In WAC ACRs are called ACLs (Access Control Lists).

WAC's [Effective ACL Resource](https://solid.github.io/web-access-control-spec/#effective-acl-resource) discovery is described in [the WAC spec](https://solid.github.io/web-access-control-spec/) as follows:

> ### Effective ACL Resource Algorithm
> To determine the effective ACL resource of a resource, perform the following steps. Returns string (the URI of an ACL Resource).
>
> 1. Let resource be the resource.
> 2. Let aclResource be the ACL resource of resource.
> 3. If resource has an associated aclResource with a representation, return aclResource.
> 4. Otherwise, repeat the steps using the container resource of resource.



## See also

- [Access Control Resource discovery](https://github.com/solid/authorization-panel/issues/228)

