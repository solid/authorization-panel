# Limiting application access while not acting as resource controller

https://solid.github.io/authorization-panel/authorization-ucr/#uc-client-constraints

### ACP

ACP requires that if `acp:agent` and `acp:client` are present in the matcher, both need to be satisfied.

```ttl
PREFIX acp: <http://www.w3.org/ns/solid/acp#>
PREFIX acl: <http://www.w3.org/ns/auth/acl#>
PREFIX ex: <https://example.com/>

ex:ReaderAgentPolicy
  acp:anyOf ex:BobMatcher, ex:AliceMatcher ;
  acp:allow acl:Read .

ex:BobMatcher
  acp:agent ex:Bob ;
  acp:client ex:Projectron .

ex:AliceMatcher
  acp:agent ex:Alice ;
  acp:client ex:Projectron, ex:OtherApp .
`ex:BobMatcher` allows access to `ex:Projectron` but only if `ex:Bob` is using it.
