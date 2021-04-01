# Trusted Apps Replacement

This proposed replacement for trusted apps details splitting it into two kinds:
 - "Signatory Change," which allows **resource owners** to change what some authorized agent (person or organizatin) has access to based on the app, bot and/or identity provider (signatory) sending the token.
 - "Client Constraints," which allows **app users** to change what an app has access to

## Terminology
 - Client User: The individual using an client
 - Resource Changeler: An individual who can set access change information for a specific resource. Under ACL, this is the individual who has `change` access, but may be different in other access change systems.
 - Trusted: A system is said to be "trusted" by someone if that individual is okay with the system performing any operation in the Solidverse as them.
 - Semi-trusted: a system is said to be "semi-trusted" by someone if that individual is only okay with the system performing a subset of all possible operations in the Solidverse as them.
 - Identity Provider (IDP): A server trusted by the app user responsible for giving access to apps.
 - Signatory: A party involved in signing a token (either the client or identity provider)

### Example Access Change Systems
For each item detailed here, I provide a few examples on how each could be represented.
 - WAC: The current access change system in Solid (https://github.com/solid/web-access-change-spec)
 - TuringAC: A hypothetical new access change system that would a Turing-Complete language (I use JavaScript for the examples but it could be anything) language to define access change rules. The need for it was proposed [here](https://github.com/solid/data-interoperability-panel/issues/34). TuringAC is not fully thought out, it is simply here as an example.
 - ShapeAC: A hypothetical new shape based access change system that would use shapes to define what an agent has access to based on the shape of the data.
 - TagAC: A hypotetical new access change system (defined by Michael Thornburgh [here](https://github.com/solid/authorization-and-access-change-panel/issues/48)) that relies on tagging resources to define the agents who have access.

## Signatory Change

The Signatory Change system allows resource owners to define the apps and identity providers that can and cannot access data.

This feature is for resource owners that are paranoid about the apps and/or identity providers that are used to contact this server. It is recommended that this feature is only used for very specific kinds of data as enabling it too often violated Solid's philosophy of being able to access data from any client.

Signatory Change should be used under the caution that there is no way to fully ensure that a client is truely what it claims to be. Spoofing and malicious user-agents are always possible. Therefore, this tool should **only** be used to prevent phishing attacks by malicious apps.

### Representing in Access Change Rules

Because blanket restrictions on resources from the signatory perspective should be discouraged in most cases, by default, resource owners are allowed to set a blacklist of signatories. However, the rules can be changed into a whitelist if a resource owner desires.

TODO/NOTE: As per Tim's request, all acl based rules should be modified to have unique RDF types such that any subset of triples on the new type will not communicate a less secure access change rule.

#### As a Blacklist

In these examples, we ban the apps at `https://evilapp.com/card#i` and `https://badguys.org/card#i` or any token created by `https://shadyidp.com` from being able to `read` `https://mypod.com/meetings/meeting1.ttl`

*TuringAC*
```javascript
(credentials, resourceIdentifier, permissionSet, store) => {
  const appBlacklist = [
    'https://evilapp.com/card#i',
    'https://badguys.org/card#i'
  ]
  const idpBlacklist = [
    'https://shadyidp.com'
  ]
  return (
    resourceIdentifier === `https://mypod.com/meetings/meeting1.ttl` &&
    permissionSet.has('READ') &&
    !(
      appBlacklist.some(appBlacklistURL => credentials.delegate === appBlacklistURL) ||
      idpBlacklist.some(idpBlacklistURL => credentials.issuer === idpBlacklistURL)
    )
  )
}
```

*WAC*
```
@prefix  acl:  <http://www.w3.org/ns/auth/acl#>.

<#authorization>
    a                  acl:Authorization;
    acl:accessTo       <https://mypod.com/meetings/meeting1.ttl>;
    acl:mode           acl:Read;
    acl:agentClass     acl:AuthenticatedAgent;
    acl:bannedClient   <https://evilapp.com/card#i>,
                       <https://badguys.org/card#i>;
    acl:bannedIDP      <https://shadyidp.com>.
```

*ShapeAC*
Let us assume that the goal is now to block the bad actors from all files containing the shape at `https://shapes.com/meeting`.

```
@prefix  acl:  <http://www.w3.org/ns/auth/acl#>.

<#authorization>
    a                  acl:Authorization;
    acl:accessTo       <https://mypod.com/>;
    acl:followingShape <https://shapes.com/meeting>
    acl:mode           acl:Read;
    acl:agentClass     acl:AuthenticatedAgent;
    acl:bannedClient   <https://evilapp.com/card#i>,
                       <https://badguys.org/card#i>;
    acl:bannedIDP      <https://shadyidp.com>.
```

*TagAC*
Let us assume that the goal is now to block the bad actors from all files tagged with `https://organization.org/tags/sprintPlanning` and that "meeting1.ttl" is tagged as "sprintPlanning".
```
@prefix  acl:  <http://www.w3.org/ns/auth/acl#>.

<#authorization>
    a                  acl:Authorization;
    acl:accessTo       <https://mypod.com/>;
    acl:tag            <https://organization.org/tags/sprintPlanning>
    acl:mode           acl:Read;
    acl:agentClass     acl:AuthenticatedAgent;
    acl:bannedClient   <https://evilapp.com/card#i>,
                       <https://badguys.org/card#i>;
    acl:bannedIDP      <https://shadyidp.com>.
```

#### As a Whitelist

In these examples, we allow the apps at `https://moralapp.com/card#i` and `https://goodguys.org/card#i` or any token created by `https://trustworthyidp.com` to `read` `https://mypod.com/meetings/meeting1.ttl`. All other clients and idps are banned.

*TuringAC*
```javascript
(credentials, resourceIdentifier, permissionSet, store) => {
  const appWhitelist = [
    'https://moralapp.com/card#i',
    'https://goodguys.org/card#i'
  ]
  const idpWhitelist = [
    'https://trustworthyidp.com'
  ]
  return (
    resourceIdentifier === `https://mypod.com/meetings/meeting1.ttl` &&
    permissionSet.has('READ') &&
    (
      appWhitelist.some(appWhitelistURL => credentials.client === appWhitelistURL) ||
      idpWhitelist.some(idpWhitelistURL => credentials.issuer === idpWhitelistURL)
    )
  )
}
```

*WAC*
```
@prefix  acl:  <http://www.w3.org/ns/auth/acl#>.

<#authorization>
    a                   acl:Authorization;
    acl:accessTo        <https://mypod.com/meetings/meeting1.ttl>;
    acl:mode            acl:Read;
    acl:agentClass      acl:AuthenticatedAgent;
    acl:bannedClient  acl:AuthenticatedAgent; # Ban all agents be default
    acl:bannedIDP       "*"^^xsd:string;
    acl:allowedClient <https://moralapp.com/card#i>,
                        <https://goodguys.org/card#i>;
    acl:allowedIDP      <https://trustworthyidp.com>.
```

*ShapeAC*
Let us assume that the goal is now to only allow the good actors to access all files containing the shape at `https://shapes.com/meeting`.
```
@prefix  acl:  <http://www.w3.org/ns/auth/acl#>.

<#authorization>
    a                   acl:Authorization;
    acl:accessTo        <https://mypod.com/>;
    acl:followingShape  <https://shapes.com/meeting>;
    acl:mode            acl:Read;
    acl:agentClass      acl:AuthenticatedAgent;
    acl:bannedClient    acl:AuthenticatedAgent; # Ban all agents be default
    acl:bannedIDP       "*"^^xsd:string;
    acl:allowedClient   <https://moralapp.com/card#i>,
                        <https://goodguys.org/card#i>;
    acl:allowedIDP      <https://trustworthyidp.com>.
```

*TagAC*
Let us assume that the goal is now to only allow the good actors to access all files tagged with `https://organization.org/tags/sprintPlanning` and that "meeting1.ttl" is tagged as "sprintPlanning".
```
@prefix  acl:  <http://www.w3.org/ns/auth/acl#>.

<#authorization>
    a                   acl:Authorization;
    acl:accessTo        <https://mypod.com/>;
    acl:tag             <https://organization.org/tags/sprintPlanning>;
    acl:mode            acl:Read;
    acl:agentClass      acl:AuthenticatedAgent;
    acl:bannedClient    acl:AuthenticatedAgent; # Ban all agents be default
    acl:bannedIDP       "*"^^xsd:string;
    acl:allowedClient   <https://moralapp.com/card#i>,
                        <https://goodguys.org/card#i>;
    acl:allowedIDP      <https://trustworthyidp.com>.
```


### General Networking Flow

Below is how token procurement and resquest process works.

Again, it is important to stress that a resource server can ensure that a token is signed by a certain identity provider, but cannot be completely sure that a token is used by a certain application. If the application and identity provider are in collusion, the token can be spoofed.

In the following scenario, Alice is going to go to `https://badguys.com` not knowing that it's a malicious site. Bob, however, does know about badguys, and has set up access change rules to prevent badguys from being able to take his data.

This deviates from the [current dpop flow](https://tools.ietf.org/html/draft-fett-oauth-dpop-03) in the following ways:

AUTHORIZATION
 - In step 7, an additional field is provided that defines the webid of the app
 - In step 8, an app webid is required
 - In step 9, the redirects in the app webid are validated
 - In step 11, the client_id is embedded in the verifiable credential

SENDING REQUEST
 - In step 9, we extract wthe client id
 - In step 10, we fail because of the applied access change rules

![](https://i.imgur.com/k1yymTI.png)


#### Authorization

##### 1. Alice navigates to badguys.com
Alice goes to badguys.com, not knowing that this is an app that will malicously use data.

##### 2. Alice Selects her WebId
Alice tells badguys.com her WebId and it is fetched. Below is Alice's WebId:
```
@prefix : <#>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix n0: <http://xmlns.com/foaf/0.1/>.
@prefix schem: <http://schema.org/>.

:me
    a schem:Person, n0:Person;
    solid:oidcIssuer <https://idp.com>,
                      <https://otheridp.com>.
```
The important thing to note here is the `solid:oidcIssuer` that links a variable number of issuers to Alice. All these issuers are trusted by Alice, but may not be trusted by other parties. In the case that a party does not trust one issuer, Alice should use a different one.

##### 3. Get OP Configuration
A request is made from `badguys.com` to get the openid configuration for Alice's issuer `idp.com`. It looks like this:
```
{  
   "issuer":"https://idp.com",
   "authorization_endpoint":"https://idp.com/authorize",
   "token_endpoint":"https://idp.com/token",
   "userinfo_endpoint":"https://idp.com/userinfo",
   "jwks_uri":"https://idp.com/jwks",
   "registration_endpoint":"https://idp.com/register",
   ...
}
```
There is more to this document, but the most important thing for the purposes of this example is the `authorization_endpoint`, which tells us where to make a request to login, and the `jwks_endpoint` which tells us where I can get the public keys for this identity provider.

##### 4. Get OP JWKS
A JWKS (JSON Web Key Set) is a set of valid public keys that represent this identity provider. These can eventually be used to prove that a token has been signed by this server. They look like this:
```json
{  
  "keys":[  
    {  
      "kty":"RSA",
      "kid":"xeOjes9u3AcU4LBzcanEM7pZLwSlxaN7U62ZzOBDQuw",
      "alg":"RS256",
      "key_ops":[  
        "verify"
      ],
      "e":"AQAB",
      "n":"oB2LgkiZZ5iLAz1d4ua7sVxdbzY2nIRkDtf4UE08mWsD6UYRzLR98_gMAfnKB8i9yPCQkxfA5w_SZq6Y7odG1qSwLHM2mb_O2GSvY9kaG00UpeeEJCR19c7Jkcmq3GXh4yujnm2TFQ6YAzYNgrXkHlusaFUApJaQN6zr4AvmR_vX_5i__Ku7nuU-GbaV75LSr8o0QANdYFF0ooz5DJvydPplF8mO9_oD7ceSNLWP1AXlFs5JH6MEhH02dELb4-zeLcVzhoqON60cABTpbYSf1lLbYZsVUQ3cYE9CxXaByY2YNuQgc0k29mSmUvwEs0hNA5xUcE3-y_qKpYKniErb9Q"
    }
  ]
}
```

##### 5. Generate Public/Private Key Pair
Badguys.com must create a public/private key pair to represent itself. This should be done in a form that is compatible with the identity provider it will be using.

##### 6. Saves the Public/Private Key Pair to local storage
Badguys, then saves this key pair to local storage so that it can be used later.

##### 7. Authorization Request
With everything prepared, the app makes an authorization request to the identity provider (seen below):

```
GET https://idp.com/authorize?
  scope=openid id_vc&
  response_type=id_token token&
  redirect_uri=https://badguys.com/&
  dpop=ey..........
  clientid=https://badguys.com/card#i
```
The main addition to the [current dpop flow](https://tools.ietf.org/html/draft-fett-oauth-dpop-03) here is that an additional field called `clientid`. This is where the app's webid should be included.

When decrypted, the dpop token looks like:
```json
header: {
  jwk: PUBLIC KEY OF THE APPLICATION,
  typ: 'dpop+jwt',
  alg: 'RS256',
}
body: {
  htu: 'https://idp.com/authorize',
  htm: 'get',
  jti: 'ajkdsfhjauilewhdjknf3uoej', // Some random identifier
  iat: 1581946330
  exp: 1581950000
}
```

##### 8. Retrieves WebID
Once the auth request is received on the the identity provider, it will make a request to the provided "clientid". Below is what badguys.com looks like:

```
@prefix : <#>.
@prefix n0: <http://xmlns.com/foaf/0.1/>.
@prefix schem: <http://schema.org/>.

:me
    a no:Agent;
    solid:webRedirect <https://badguys.com/>;
    solid:iosRedirect "badguys://"^^xsd:string.
```

Notice that instead of having issuers, app webids have redirects. These redirects can be used to confirm that an app at a certain location is truely the app identified by this WebId.

##### 9. Validates RedirectURL with WebID
If the identity provider sees a mismatch between the provided `clientid` and the redirect, it must reject.

##### 10. Gets Alice's Consent
The IDP issues an auth challenge to Alice and Alice passes it.

##### 11. Generates an id_vc
The idp generates and signs a verifiable credential. When when unsigned, it looks like this:
```json
{
  "sub": "https://alice.com/profile/card#me",
  "iss": "https://idp.com",
  "aud": "https://badguys.com",
  "client_id": "https://badguys.com/card#i"
  "iat": 1541493724,
  "exp": 1573029723,
  "cnf":{
      // DPoP public key confirmation
      "jkt":"0ZcOCORZNYy-DWpqq30jZyJGHTN0d2HglBV3uiguA4I"  
  }
}
```
The main important thing to note is the `client_id` field is the WebID of the application. 

##### 12. Returns returns to redirect URL
The token is returned to the redirect url

#### Sending a Request

##### 1. Creates DPoP Token
A dpop token is generated for a request to Bob's pod. A new DPoP token is generated for each request. Unsigned, it looks like:
```json
header: {
  jwk: PUBLIC KEY OF THE APPLICATION,
  typ: 'dpop+jwt',
  alg: 'RS256',
}
body: {
  htu: 'https://bobpod.com/resource',
  htm: 'get',
  jti: 'ajkdsfhjauilewhdjknf3uoej', // Some random identifier
  iat: 1581946330
  exp: 1581950000
}
```

##### 2. Request sent
The request is sent with the following authorization headers:
```
authorization: DPOP ey.... <-- The id_vc retrieved from the idp
dpop: ey... <-- The generated DPoP token
```

##### 3. Checks pop_token audience
If the `htm` or `htu` does not match the method and url of the request, the server must reject.

##### 4. Checks client signature
If the dpop token does not have a matchign signature with jwk inside the auth token, the server must reject.

##### 5. Retrieves Subject Profile
Using the `sub` claim of the auth token, the server retrieves Alice's profile.

##### 6. Checks Issuer
If the issuer of the auth token is not among the issuers listed in Alice's profile under `solid:oidcIssuer`, the server must reject.

##### 7. Retrieves OP Configuration
The server fetches the configuration of the IDP in order to get its JWKS

##### 8. Requests JWKS
The server fetches the JWKS and validates that the id_vc was signed by it.

##### 9. Performs Authentication
The pod extracts the id and the client from the id_vc.

##### 10. Performs Authorization
The pod applies the extracted credentials against the access change rules. In this case, we reject because we Bob has banned `https://badguys.com`

##### 11. Returns Result
The server return the result. In this case a 403 is provided denoting the forbidden nature of the request.

## App Constraints

// TO BE COMPLETED.
// In my opinion, this is actually the most important section of this proposal, but we're still working through a few things conceptually.
