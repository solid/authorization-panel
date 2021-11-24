# Limiting application access while not acting as resource controller

https://solid.github.io/authorization-panel/authorization-ucr/#uc-client-constraints

### ACP

ACP allows combining multiple matchers with `acp:allOf`, this allows expressin policy which uses `acp:agent` and `acp:client` in combined matchers.

```ttl
PREFIX acp: <http://www.w3.org/ns/solid/acp#>
PREFIX acl: <http://www.w3.org/ns/auth/acl#>
PREFIX ex: <https://example.com/>

ex:accessControlResourceA
  acp:resource ex:resourceX ;
  acp:accessControl ex:accessControlB .

ex:accessControlB acp:apply ex:PolicyC .

ex:PolicyC
  acp:allOf ex:agentMatcherD, ex:clientMatcherD ;
  acp:allow acl:Read .

ex:agentMatcherD
  acp:agent ex:Bob .

ex:clientMatcherD
  acp:client ex:Projectron .
```

`ex:Policy` allows acces to `ex:Projectron` but only if `ex:Bob` is using it.
