![ecosystem diagram](https://raw.githubusercontent.com/solid/app-authorization-panel/master/images/webid-dpop.png)

## Verifying Application Identity

* Authorization Servers MUST verify app identity (WebID), and include it in JWT issued as *access token*. *TODO: define mechanism - `redirect_uri`, public key for secure clients*
* Resource Servers SHOULD rely on the app identity included in *access token* and SHOULD NOT try to further verify it.

## Presenting User's Application Authorization to Resource Server

* Authorization Server MUST make available to the Application all the Authorizations granted by the User to that Application.
* Application MUST communicate in the request to the Resource Server, any IRI denoting Authorization that grants that Application access to requested resource. *TODO: choose mechanism like HTTP Header or DPoP Proof or DPoP-bound Access Token (if issued per RS)*

## Consent Screen

* Authorization Server MUST present User with Consent Screen to grant Authorizations to the Application. Authorization Server SHOULD display on Consent Screen information from Application's WebID Profile.