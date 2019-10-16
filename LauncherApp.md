# Launcher App

The Launcher App is the most trusted App in the users App ecosystem. It allows the user to add Apps to it's preferred list of Apps, keeps keys for each App, and signs tokens or [HTTP Sig headers](https://github.com/solid/authentication-panel/blob/master/HttpSignature.md) for an App that allow an App to authenticate to a remote servers. As such it is at the center of Access Control and Authentication Logic.

In more detail. The App Launcher:
  * presents the various Apps a user likes to use in a user friendly way, and makes it easy for a user to add new ones, and to launch them.
  * For each App the user adds, it creates an AppID Document identifying the App instance as used by a particular user
  * If needed maintains the link chain from the user's WebID document to it, to allow access control rules to be allow users to authenticate via their App.
  * The AppID Document keeps metadata about the App:
	  * an icon to display to the user
     * where the source code of the App is located, by linking to an App Manifest, which contains information about who built the App, the issue database for it, etc...
	  * information about restrictions on where the App can authenticate to and what it can do 
  * It creates a local space for each App to use and sets the initial access rights to that space
  * It plays the role of the keychain for each App, by creating public/private key pairs for each App securely, publishing the public key and managing links from the user's WebID to the App instance profiles. It could also play as the OpenID Connect proxy.
  
See [issue 45: User Controlled Authorization App and App Launcher](https://github.com/solid/authorization-and-access-control-panel/issues/45) 


# Description

The most important role of the launcher App is to allow apps to authenticate to remote Origins. Because it can launch other Apps in new Frames it can communicate with them securely using [Window.postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage). As it launches them it can sign headers and identify the App instance that is making the request. But because it is signing the request, and not the App, it can speak for the App instance. If the App itself were to sign tokens, it would not be possible for a resource server to distinguish between an App instance and any other, since any key could be leaked by an App to its origin and beyond. By maintaining the keys, publishing public keys, and linking them to the users WebID, the App Launcher create an identity for a particular App. 

The key part of the Launcher App is enabled by [Window.postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) API available in the browsers. This is illustrated in the following diagram:

![Launcher App](https://user-images.githubusercontent.com/124506/66335877-3485bc00-e93c-11e9-9861-b19beefea18e.png)

This shows two Apps running side by side in the users' browser on her laptop. The left one is the App Launcher and the right one is a Calendaring App. (These are shown side by side to help describe the information flow in Red, not as a prescription for how the UI should be built). The App Laucher's Origin is alice's Freedom Box. The App to the right has for Origin an App server at Apps.com. And we show the Calendar App wanting to communicate with Alice's friends Bob's server. Presumably after a 401 trying to access a resource on `bob.me` 
1. the calendar App requests the App launcher to sign some headers using [Window.postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) 
2. the App Launcher verifies that the access is allowed and signs the headers if [HTTP-Sig](https://github.com/solid/authentication-panel/blob/master/HttpSignature.md) is allowed 
3. The Calendar App sends the request with the needed headers to `bob.me`.
 

In order to be able to develop some examples we put forward the following sketch of an ontology

```Turtle
solid:App a owl:Class;
   rdfs:comment """
	  An App Instance on a user's Pod, that contains links
	  to the source code, manifest, doc, ...
	  and links to the local preferences, public keys, 
	  space allocated for a pod, ...
	""".

solid:manifest a rdf:Propert;
   rdfs:domain solid:App;
	rdfs:range solid:Manifest;
	rdfs:comment """
	   relates an App instance description in control of the user to the App Manifest, that contains links
		to its source, owners, ...
	""";
```


# Problems Solved

The Launcher App solves the following problems [identified by the Panel](https://github.com/solid/authorization-and-access-control-panel/blob/master/meetings/2019-10-09.md), see also [updated version in HackMD](https://hackmd.io/GeD5XLyzTG6WNHsxkOxqJA)

### Identifying the App Source & App Manifest

  The App Launcher keeps track on the user's pod server
of links to the App Manifest (which is probably where the source is to be found)

For example a Calendar App could maintain information such as the following:

```Turtle
<#CalendarApp> a solid:App;
   solid:manifest <http://calendar.app/manifest>;
   solid:storage </apps/calendar/>;
   cert:key </keys/k42#> .
```

### Constraining the App 

The App launcher can keep information about desired restrictions on the app, placing those in a special file
that can access controlled for only LauncherApp visibility.

```Turtle
<#CalendarApp> a solid:App;
    solid:aclRestrictions <restrictions>;
```

Because all access tokens, or signatures go through the
LauncherApp it can refuse to give out such tokens if for
resources that are not allowed.

### App asking for access to a specific resource

If the header of the resource that is not allowing access has a link to an ldp:Container to make such a request, then the App can make a request using the WebID of the App - the AppId. It can use the launcher App to sign such
a request (or could the Launcher App could make the request itself on behalf of the launched app?)

### App asking access for the user

Same as above but the request now contains the WebID of the user, or some way of identifying the user (say via 
a keyId URL).

### An App should be able to ask access to a kind of resource

Same as above, but in addition the App can specify
the type of resource it wants access to. This will
require changes to ACLs.

**todo**: how would that work for ACLs?

### Varying levels of sensitivity for resources

It would help here if the Launcher App was proxying the
requests as it could then be the intermediary that opens up a Pop Up Box when a resource declares itself to be of
a certain type of sensitivity

## Additional Problems to be solved

### Access Control Policies

The Launcher App could be tuned for policies as to when
to automate the access control decisions, so as to avoid
overhwelming the user with pop up boxes. This would work
especially well if the Launcher App proxies all HTTP requests.

### Identify the App

Because the App Launcher has a pointer to the App Manifest and launches the App according to that information it can have a well recognised way of 
identifying an App that is much more fine grained than
an HTTP Origin.
 
# Use Cases

The Launcher App adderesses the following use cases from the [Use Case Document](https://github.com/solid/authorization-and-access-control-panel/blob/master/UseCases.md)

### Alice's Admin App

> Alice uses https://admin.example to control her pod
Admin wants to be able to read and write to all files on a user’s Pod

This is the App we are calling the Launcher App. The first thing
it needs to do is to be copied to the Pod Owner's Origin in order to avoid leakage of public/private key pairs to other origins. Due to current limititions of rights given by browsers to apps with the same origin, the origin may even need to be a subdomain such as `safe.user.me`, for a pod at `user.me`. It can authenticate with a username/password, the user's WebID, or other methods. 

The Launcher App authenticating as the user is given administrative rights by editing an ACL. One way to do this is to allow ACLs 

1. to include other ACLs via an include statement
2. to allow acls to describe groups of resources using some form of regular expression based on [Powder](https://www.w3.org/TR/2009/NOTE-powder-primer-20090901/).

This would then require only one ACL to give the Admin App full rights such as with 
*
```Turtle
[] acl:accessToClass [ acl:matching "https://*.user.me/**" ];
   acl:mode acl:Read, acl:Write;
   acl:agent <https://safe.user.me/apps/admin#>;
```

Then if all acls automatically include this acl, all resources can automatically give access to the Admin App.

### Alice's Single Player Game

> Alice uses https://simplegame.example to play a singleplayer game
>   * Simplegame only needs some file somewhere that it can write its own configuration to. It does not care where it is
>   * Simpleapp will also need to access this file again even if it’s being used on another machine


1. Alice discovers the new game at https://simplegame.example 
2. She Drags and Drops the URL onto her Launcher App (or even may have a button to do this), which then follows the procedure
    1. The Laucher App fetches the representation at the Link and receives a Manifest
    2. The Manifest describes the game, has an icon, links to the start page, describes window size, ... information the Launcher App uses to present the SinglePlayer App to the user.
	3. If the user requests for it to be added, the Launcher App - which is the most trusted App and comes from the Pod Owners' own Origin - and has access in read/write to the user's Pod, creates an LDPC in the appropriate place for the App to store its files.
	4. The Launcher App creates 
	   * a public/private key in the browser and publishes a description of that key in a KeyId Document somewhere
		* it publishes a document to an LDPC describing the Single Player Game with links to the manifest, html, a link to the public key, and whatever else is needed ... thereby giving the App instance for that user an App Id
		* Directly or indirectly there is a link from the user's WebID to AppId of the game instance
3. When Alice launches the Single Player App by clicking on its icon, the App Launcher opens a new window or frame with as startup link the URL for the html that starts the App, and information about where to send any messages using [Window.postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) and where to store its local state, which for the purpose of the SinglePlayer App can be a space with very limited access rights.
	* When the Single Player App wishes to create files in that space, it asks the Laucher App via Window.postMessage to sign a token or a set of HTTP headers to authenticate as that single player instance.
	* The App Launcher knowing which frame the Single Player App is in will know which private key to choose to sign the headers. It may also know that this App should only authenticate to that specific space, and so will not sign any headers for requests to other origins.
		

### Alice Social Photo App (sPhoto) 

> Alice uses https://decentPhotos.example to view her photos and her friend Bob’s photos
>  * Decent photos wants to read to all photos on Alice’s Pod
>  * Wants to read all photos on Bob’s Pod that Alice has access to

We extend this example to a friend group, as there is no reason not to make it scalable. 

After installation the Launcher/Admin App gives access to sPhoto to a subfolder structure where Alice stores her Photos. The Launcher App initially only signs requests for files in that URL space.

As Alice grows confident in the quality of the App she extends the permissions by using the Launcher App to edit any photos of members of a particular group by WebID, from which it can find the spaces in which photos are located. Whenever access control is required there the sPhoto sends a request to the Launcher App, 
which signs the tokens if allowed, without bothering Alice for authorization at each point.

We note here that if the Launcher App could control all the web connections (as would have been possible with [Foreign Fetch](https://developers.google.com/web/updates/2016/09/foreign-fetch) this would give it a lot more control. Indeed there may be a lot to be gained by giving the Launcher App control over the RDF Quad store of all data found on the web.

If ever Alice sPhoto edits a photo outside of the well know space and needs Write Access to an external resource, the Launcher App gives it out but not before asking Alice for permission.


# Success Criteria

The Launcher App can be evaluated against the [Success Criteria](https://github.com/solid/authorization-and-access-control-panel) identified by the panel.


### The system is not abusable

More criteria on how to evaluate abuse are needed

### An App can request access to a specific resource

presumably a protected one.
Yes, it can ask the Launcher App for a signature, and it can 
decide according to policies whether to sign or not.

### An App can request access to a specific type of data without knowing the structure of resources on a Pod

yes, if given a URL it can make a request on it.

### Access requests can be sent when the resource owner is not present to be approved once the user is present

This is not a proposal for Access Requests. 
It can work with one though.

### Apps can request the ability to write a specific type of data and will be told where it should write it

Potentially if the Launcher App keeps more information about where things are it can respond to queries on data it has found that the app is allowed access to and give it the URLs for that data or that data.

### It should be possible for an agent to block/allow certain apps from accessing a specific resource as that agent

Either the Resource Server blocks the App because it does not provide the right credentials, or the AppLauncher refuses to sign because the App is requesting access to content which are in the disallowed region.

### It should be possible for an agent with Control access to block/allow certain apps from accessing a specific resource as any agent

Yes, the Launcher App (a.k.a Admin App) can do that as it controls the authentication keys for each App.

### It should be easy to allow others accessing your resources to use apps you're okay without requiring your explicit consent.

wACLs should try to keep open about what apps have access if they want to be able to cooperate easily with other organisations.

### Access to specific types of data should extend to new resources that contain that data

An wACL for an agent could be 

```Turtle
[] acl:accessToClass [ mime:type "photo/*" ];
   acl:mode acl:Read;
   acl:agent <https://user.me/apps/sPhoto#> .
```	

And this could be referenced or included from any wACL for any
resource that wants to follow those rules.

### Access to specific types of data should not expose other data that was not requested

It is too early in the life cycel of this proposal for this to be determined.

### Data should have different levels of requirements for user's conciousness in consent

Good idea. This could then be used by the Launcher App to affect
certain parts of the UI before signing content.
