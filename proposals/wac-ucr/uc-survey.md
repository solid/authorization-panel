Add your name; add +1/+0/0/-0/-1 to a use case you find interesting and add a comment along these lines:

* +3 "Needed. Implemented. In use"
* +1 "yes, I need it, will implement, worth doing"
* +0 "I'm kind of for it, but can live without it"
* 0 "I really don't care about it"
* -0 "I'm kind of against it, but can live with it"
* -1 "I'm against this right now" (provide an explanation)

---

Notes:
* [ai] application implementation
* [si] server implementation
* csarven:
  * [d] Will implement or implemented in [dokieli](https://github.com/linkeddata/dokieli).
  * [ap] Authoring and publication tools.
  * [sw?] What common social Web use cases are currently out there for this?

---
# Current Solid WAC features

## Basic resource access

### Control access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-control

* +1 justinwb: to the ability to read and write permissions on resources. public [ai]. private [ai].
* +1 elf-pavlik: 
* +1 csarven: Wide use. Any agent having the ability to change access controls has wide use. Required for [ap]. [d]. public [ai].
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin: public [si].
* +1 bblfish: public [si].

### Read-write access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-write

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap]. [d].
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin:

### Read-append access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend

* +3 timbl:
* +1 justinwb:
* +1 elf-pavlik:
* csarven: The resume resource is assumed to be simple ie. a single unit, and there is no assumption about it being a dynamic or compound resource. UC #basic-readappend assumes an append operation that can add information to a specific part of a resource ie. the references section. An agent can append information to a resource (as in #basic-appendonly) but there is no guarantee or validation assumed to take place as part of the use case - hence, other requirements needs to be met in parallel for this use case to actually work.
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +0 dmitrizagidulin:

#### Alice stores Danielle's recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-single-storage

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Generally worth doing. This UC implies a requirement that ensures data integrity.
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix "Im not sure the use case makes sence. Seems more usefull if there is a list to add to of some kind, not freely add but no change. But OKm could be a case."
* +1 hindia:
* +0 dmitrizagidulin:

#### Danielle stores their own recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-multi-storage

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: [d] such that agent A sends a notification about the recommendation to agent B's inbox ie. #collection-readappend , instead of updating a resource that references it.
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +0 dmitrizagidulin:

### Append-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-appendonly

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Convoluted scenario but the use case has wide use. [d] with some variations.
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* -0 dmitrizagidulin: to non-container Append-only
    access separate from Read access (since the server-side PATCH engine often
    needs Read access to process the file and prevent collisions)

### Removing access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-removing

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. This UC is possibly a duplicate of #basic-control. [d].
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

### Read-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readonly

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap]. [d].
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

### Group access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-group

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: This should be core but can live without it. May [d]. Group management may not be common for [ap].
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin:

### Public access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-public

* +1 justinwb:
* +1 csarven: Wide use. Required for [ap]. [d].
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

### Logged in access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-authenticated

* +0 justinwb: There's controversy over whether this can be valuable given
the ability for anyone to spin up their own identity in a decentralized
system. I think that in concert with the ability to specify or verify
trusted issuers this can be valuable and probably beneficial in a number of
scenarios.
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap]. [d].
* +1 jaxoncreed:
* +3 timbl: Please link to the contrversy if you cite it. Allowing non-anonymous public acesss is a really important level.
a bit like Anonymous FTP - anyone can access it but please leave your email address  It is not abut being secure
agaist being accessed by evil people. It is about being to collect a list of interested people.
I know people push back on this because they don't understand it.  A huge number of things
of web you can log in with any email address or any facebook or google account.
This is an important aspect of onboarding and the growth os Solid.
* +1 KaiGilb: graphMetrix
* -0 hindia: because I dont like the idea of "forcing" people to register but i can see a use case in a tight ship
* -0 dmitrizagidulin: because at best, it may lead to the creation of throwaway / temporary
    Solid accounts that will then gather dust and inflate numbers. -1 if this feature is explicitly
    intended for list collection. For that, we should have a separate use case, something like
    "Grant access only if the user consents to be added to a list for future contacting etc".

## Basic collection access

### Read-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readonly

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap]. [d].
* +0 jaxoncreed: seems like a weak example, but it's needed for other examples
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

### Read-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readwrite

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap]. [d].
* -0 jaxoncreed: Some things need to be clarified here. If you have write access to a collection, do you also have write access to the resources of the collection? It wouldn't make sense for someone to be able to remove a resource from the collection if they don't have write access to that resource. Yet, on the other hand, if having read access on the collection doesn't give you read access to the resources, then it would be incongruent to allow write access to a collection to extend to the resource.
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin: (though I agree with @jaxoncreed that clarification is needed.)

### Read-append access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappend

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap]. [d].
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +0 dmitrizagidulin: The usefulness of this UC, as written, depends on resource
    names (filenames) being human-readable.

### Read-append-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappendwrite

* +1 justinwb: Being able to designate the creator of a resource with specific
privileges in an append scenario on a container is extremely important to
a number of collaborative scenarios.
* +1 elf-pavlik:
* 0 csarven: Seems like duplicate of #collection-readwrite and #collection-readappend
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* -0 hindia: sounds a bit dangerous
* +1 dmitrizagidulin: I think this points to the need for another WAC primitive.
    Something like "allow/disallow authors to edit the comments they've submitted",
    but more general.

### Append-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-appendonly

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap] eg. creating annotations or notifications. [d].
* +1 jaxoncreed:
* +3 timbl: Append-only access allows you to implement the semantics of message passing.  That is a crcuial building blcok for many systems,
technical and social.  We may been extra functionality to in some cases giuve people read-write access to a thing they have posted using append-only access.
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

### Control access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-control

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Any agent having the ability to change access controls has wide use. Required for [ap]. [d].
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

## Inheritance

### Read-only access to collection of resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readonly

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap]. [d].
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

### Read-append access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readappend

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap] eg. need to set base access to read and share annotations and notifications. [d].
* 0 jaxoncreed: I think this needs more clarification on what happens to nested collections.
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin:

### Read-write access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readwrite

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap] - similar to #inheritance-readappend. [d].
* 0 jaxoncreed: I think this needs more clarification on what happens to nested collections.
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

### Append-only access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-appendonly

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap] ie. to be able to update existing resources or to create new ones in a container. [d].
* 0 jaxoncreed: I think this needs more clarification on what happens to nested collections.
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

### Control access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-control

* +1 justinwb:
* +1 elf-pavlik:
* +1 csarven: Wide use. Required for [ap]. [d].
* 0 jaxoncreed: I think this needs more clarification on what happens to nested collections.
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +3 dmitrizagidulin:

### Default permissions on created resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-defaultcreated

* +1 justinwb:
* +1 elf-pavlik:
* 0 csarven: Difference between default and inherited is not particularly significant for [ap]  - one is sufficient for most common use [d].
* +1 jaxoncreed:
* +3 timbl:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* 0 dmitrizagidulin: agree with @csarven, not sure there's enough of a difference from inherited.

### Default permissions for extended network
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-extended

* +1 justinwb:
* +1 elf-pavlik:
* 0 csarven: Not core but probably necessary for some application domains.
* +1 jaxoncreed: Just change "extended network" to "a group" to clarify.
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin:

### Adding new subjects to inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-adding

* +1 justinwb: Without an extensible inheritance system, it is near impossible
to do any permission management that doesn't require managing permissions
for every resource specifically, lest they negate permissions that were set
above.
* +1 elf-pavlik:
* +1 csarven: May be duplicating simpler use cases. May [d].
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix For the mentioned use case, it seems easier to me to give Celeste permission on the specific note directly. But being able to add new subjects sounds useful
* +1 hindia:
* +1 dmitrizagidulin:

### Modifying inherited permissions for existing subjects
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-modifying

* +1 justinwb: Without an extensible inheritance system, it is near impossible
to do any permission management that doesn't require managing permissions
for every resource specifically, lest they negate permissions that were set
above.
* +1 elf-pavlik:
* +1 csarven: May be duplicating simpler use cases. May [d].
* 0 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin:

### Forcing inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-forcing

* +1 justinwb: Some permissions shouldn't be contradicted. For example,
the administrator with full control access of a given storage (i.e. pod)
shouldn't be able to have their permissions denied by someone with control
access to a collection far down in the hierarchy so they can hide things
inside.
* +1 elf-pavlik:
* 0 csarven: Could be useful for [ap]. May be implemented in [d] depending on other available features. See also note on #collection-control.
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin:

## Conditional access

### Conditional access by time
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-time

* +1 justinwb: Setting timeouts can really come in handy, and is a good
way to ensure that permissions meant to be short-lived don't hang around
longer than they're needed. Also very useful when used in concert with
invitiation flows.
* +1 elf-pavlik:
* 0 csarven:
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin:

### Conditional access by tag
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-tag

* +1 justinwb: Noting that we haven't specified a tag-based system for data
yet in solid, the ability to tag individual resources, or collections of
resources, and then have conditions in ACLs based on those tags, can
be extremely useful. For example, being able to tag photo albums as
work-related, and then giving conditional access to only the work related
photo albums in my media library to colleagues.
* +1 elf-pavlik:
* 0 csarven: Too specific. Complicated use case. [sw?]
* 0 jaxoncreed:
* 0 timbl: Suspect this should be part of a rule based system
* +1 KaiGilb: graphMetrix
* +1 hindia:
* 0 dmitrizagidulin: The tagging mechanism is unclear/undefined.

### Conditional access by relationship
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-relationship

* +1 justinwb: Cannot underscore how important this use case is to doing
intuitive authorization on more complex data objects. We cannot rely on
physical containment alone, because by definition solid is meant to connect
disparate data together into logical views that people can work with.
For example, if Alice wants to give Bob access to Project Y, and Project Y
is comprised of tasks, milestones, calendar events, contacts, and other
things stored in different places, how can she do that rationally? How can
she ensure that directive is maintained as new things are added? This use
case addresses that.
* +1 elf-pavlik:
* 0 csarven: Way too specific. Prefer a general form of this UC eg. matching a data shape. [sw?]
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix. This seems very important. Like only give certain doctor relations access to records.
* +1 hindia:
* +1 dmitrizagidulin:

### Conditional access by filter
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-filter

* +0 justinwb: This is important, but I think we can hit it in the next cycle.
Ultimately, we need the manage access to data within the resource. There are
some rational ways to do this using machinery we already have.
* +1 elf-pavlik:
* 0 csarven: Too specific. May require querying. Alternative based on simpler use cases: hide references to resources from a container's index from agents that do not have read permission. [sw?]
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin:

### Conditional control boundaries
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-control

* +0 justinwb:
* +1 elf-pavlik:
* 0 csarven: Seems too convoluted for wide use. [sw?]
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* 0 dmitrizagidulin: tagging mechanism unclear/not defined.

### Conditional access by action
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-action

* 0 justinwb:
* +1 elf-pavlik:
* 0 csarven: May be generally useful for [ap]. May [d].
* 0 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +0 dmitrizagidulin:

### Conditional access by payment
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-payment

* +0 csarven: Generally useful for [ap] in that it can handle server's response to client error eg. prompt user with "Payment Required". May [d].
* +0 Kai Gilb: graphMetrix. Sounds cool, but maybe it could be handled by other means integrating with pay wall etc.
* +1 hindia:
* +0 dmitrizagidulin: The proof of payment could be handled by https://solid.github.io/authorization-panel/wac-ucr/#capabilities-vc

## Permissioning Applications

### Limiting access to trusted applications
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-trusted-applications

* +1 justinwb: Limiting access to only specific applications, identified
by AppID, with the caveat that the effectiveness is specifically when
the user can strongly identify the application they are using (in
piloted scenarios)
* 0 csarven: it depends on the environment of the application.
* +1 jaxoncreed:
* KaiGilb: im a little unclear on this case
* +1 hindia:
* +1 dmitrizagidulin:

### Limiting application access while not acting as resource controller
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-client-constraints

* 0 csarven: A bit of a low-level server-side plumbing. Unclear how an application (like [ap]) may want/need to set a policy as such.
* KaiGilb: im a little unclear on this case
* +1 hindia:
* +1 dmitrizagidulin:

### Application determining access privileges
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-client-determine-access-privileges

* +1 csarven: Required for [ap]. [d].
* +1 hindia:
* +1 dmitrizagidulin:

## Privacy

### Limiting access to who else is permitted
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-whopermitted

* +1 justinwb: Privacy in this context in paramount, unless the controller
specifically wants the information to be divulged.
* +1 elf-pavlik:
* +1 csarven: Duplicate of #basic-readonly.
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia:
* +1 dmitrizagidulin:

### Limiting access to other authorization conditions
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-historyofchanges

* 0 csarven: Complex use case. Perhaps change focus to auxiliary resources.
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix. Im not sure this should be handled by permission, but yes, they should not see it.
* +1 hindia:
* +1 dmitrizagidulin:

### Minimal Credential Disclosure
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-minimalcredentials

* +0 justinwb: This scenario is legitimate though I believe we'll likely
get to VC in the next cycle.
* +1 elf-pavlik:
* +0 KaiGilb: graphMetrix Sound good. I would again think this would be app controlled
* +1 hindia:
* dmitrizagidulin: This is out of scope for authorization. At best, it belongs
    in a wallet spec or implementation guide.

### Limiting access to other authorization conditions
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-historyofchanges

* +1 justinwb: Privacy in this context in paramount, unless the controller
specifically wants the information to be divulged.
* +1 csarven: Wide use. I don't plan to implement it.
* +1 jaxoncreed:
* KaiGilb: this seems like a duplicate from 2 up
* +1 hindia:

### Limit information disclosure through URI
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-limituri

* 0 justinwb: I'm not positive this is a use case for the authorization
system as much as how the resource server itself organizes and/or
represents data.
* +1 elf-pavlik:
* +1 csarven: I don't plan to implement it.
* +1 jaxoncreed:
* +0 KaiGilb: graphMetrix. Sounds good for the future, we are not there quite yet.
* +1 hindia:
* +0 dmitrizagidulin: Doesn't belong in authorization, this should be in the
    server spec security/privacy considerations section.

## Trust

### Only trust certain issuers of identity
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-trustedissuers

* +0 justinwb: Makes the authenticated agent use case more reasonable to me
* +1 elf-pavlik:
* +1 csarven: Wide use. I don't plan to implement it.
* +1 jaxoncreed:
* +0 KaiGilb: graphMetrix
* +1 hindia:
* -0 dmitrizagidulin: Seems like a very specific use case (a special case of
      allowing access by group).

### Block access to agents
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-blockagents

* +1 dmitrizagidulin: Seems reasonable.
* +1 csarven: Wide use re: support healthy community and debate. Useful for [ap]. May [d].

## Validation
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-validation

* +1 justinwb: This just seems like good engineering practice to me
* 0 csarven: Not sure why this UC is here or what's expected of it other than making sure server only processes valid authorization policies and in its absence all access is denied.
* +1 jaxoncreed:
* +1 hindia:

## Capabilities

### Possession of a verifiable credential
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-vc

* +0 justinwb: Definitely seems to be a key part of the future of access
control for us.
* +1 elf-pavlik:
* +1 csarven: Wide use.
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix
* +1 hindia: love it!
* +1 dmitrizagidulin: With the caveat that verifiable credentials (VCs) alone should
    not be used for authorization. This is an important use case, and VCs can
    be exchanged for [Authorization Capabilities (zCaps)](https://w3c-ccg.github.io/zcap-ld/)

### Possession of a link
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-link

* +0 justinwb: Can be extremely beneficial in scenarios like invitation
flows or one-time shares.
* +1 elf-pavlik:
* 0 csarven: Some use but not essential (so, -1 wrt prioritisation)
* +1 jaxoncreed:
* +1 KaiGilb: graphMetrix (maybe +1)
* +1 hindia: love this too
* +1 dmitrizagidulin: Essential for shares.
