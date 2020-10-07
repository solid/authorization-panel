Add your name; add +1/+0/0/-0/-1 to a use case you find interesting and add a comment along these lines:

* +1 "yes, I need it, will implement, worth doing"
* +0 "I'm kind of for it, but can live without it"
* 0 "I really don't care about it"
* -0 "I'm kind of against it, but can live with it"
* -1 "I'm against this right now" (provide an explanation)

---

Notes:
* csarven:
  * [d] Will implement or implemented in [dokieli](https://github.com/linkeddata/dokieli).
  * [ap] Authoring and publication tools.
  * [sw?] What common social Web use cases are currently out there for this?

---

## Basic resource access

### Control access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-control

JUSTINWB: +1
+1 elf Pavlik
* +1 csarven: Wide use. Any agent having the ability to change access controls has wide use. Required for [ap]. [d].

### Read-write access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-write

JUSTINWB: +1
+1 elf Pavlik
* +1 csarven: Wide use. Required for [ap]. [d].

### Read-append access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend

JUSTINWB: +1
+1 elf Pavlik
csarven: The resume resource is assumed to be simple ie. a single unit, and there is no assumption about it being a dynamic or compound resource. UC #basic-readappend assumes an append operation that can add information to a specific part of a resource ie. the references section. An agent can append information to a resource (as in #basic-appendonly) but there is no guarantee or validation assumed to take place as part of the use case - hence, other requirements needs to be met in parallel for this use case to actually work.

#### Alice stores Danielle's recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-single-storage

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Generally worth doing. This UC implies a requirement that ensures data integrity.

#### Danielle stores their own recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-multi-storage

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: [d] such that agent A sends a notification about the recommendation to agent B's inbox ie. #collection-readappend , instead of updating a resource that references it.

### Append-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-appendonly

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Convoluted scenario but the use case has wide use. [d] with some variations.

### Removing access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-removing

JUSTINWB: +1
+1 elf Pavlik
+1: csarven: Wide use. This UC is possibly a duplicate of #basic-control. [d].

### Read-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readonly

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap]. [d].

### Group access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-group

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: This should be core but can live without it. May [d]. Group management may not be common for [ap].

### Public access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-public

JUSTINWB +1
+1 csarven: Wide use. Required for [ap]. [d].

### Logged in access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-authenticated

JUSTINWB +0 - There's controversy over whether this can be valuable given
the ability for anyone to spin up their own identity in a decentralized
system. I think that in concert with the ability to specify or verify
trusted issuers this can be valuable and probably beneficial in a number of
scenarios.
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap]. [d].

## Basic collection access

### Read-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readonly

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap]. [d].

### Read-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readwrite

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap]. [d].

### Read-append access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappend

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap]. [d].

### Read-append-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappendwrite

JUSTINWB: +1 - Being able to designate the creator of a resource with specific
privileges in an append scenario on a container is extremely important to
a number of collaborative scenarios.
+1 elf Pavlik
0 csarven: Seems like duplicate of #collection-readwrite and #collection-readappend

### Append-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-appendonly

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap] eg. creating annotations or notifications. [d].

### Control access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-control

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Any agent having the ability to change access controls has wide use. Required for [ap]. [d].

## Inheritance

### Read-only access to collection of resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readonly

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap]. [d].

### Read-append access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readappend

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap] eg. need to set base access to read and share annotations and notifications. [d].

### Read-write access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readwrite

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap] - similar to #inheritance-readappend. [d].

### Append-only access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-appendonly

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap] ie. to be able to update existing resources or to create new ones in a container. [d].

### Control access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-control

JUSTINWB: +1
+1 elf Pavlik
+1 csarven: Wide use. Required for [ap]. [d].

### Default permissions on created resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-defaultcreated

JUSTINWB: +1
+1 elf Pavlik
+0 csarven: Difference between default and inherited is not particularly significant for [ap]  - one is sufficient for most common use [d].

### Default permissions for extended network
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-extended

JUSTINWB: +1
+1 elf Pavlik
+0 csarven: Not core but probably necessary for some application domains.

### Adding new subjects to inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-adding

JUSTINWB: +1 - Without an extensible inheritance system, it is near impossible
to do any permission management that doesn't require managing permissions
for every resource specifically, lest they negate permissions that were set
above.
+1 elf Pavlik
+1 csarven: May be duplicating simpler use cases. May [d].

### Modifying inherited permissions for existing subjects
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-modifying

JUSTINWB: +1 - Without an extensible inheritance system, it is near impossible
to do any permission management that doesn't require managing permissions
for every resource specifically, lest they negate permissions that were set
above.
+1 elf Pavlik
+1 csarven: May be duplicating simpler use cases. May [d].

### Forcing inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-forcing

JUSTINWB: +1 - Some permissions shouldn't be contradicted. For example,
the administrator with full control access of a given storage (i.e. pod)
shouldn't be able to have their permissions denied by someone with control
access to a collection far down in the hierarchy so they can hide things
inside.
+1 elf Pavlik
+0 csarven: Could be useful for [ap]. May be implemented in [d] depending on other available features. See also note on #collection-control.

## Conditional access

### Conditional access by time
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-time

JUSTINWB: +1 - Setting timeouts can really come in handy, and is a good
way to ensure that permissions meant to be short-lived don't hang around
longer than they're needed. Also very useful when used in concert with
invitiation flows.
+1 elf Pavlik
+0 csarven:

### Conditional access by tag
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-tag

JUSTINWB: +1 - Noting that we haven't specified a tag-based system for data
yet in solid, the ability to tag individual resources, or collections of
resources, and then have conditions in ACLs based on those tags, can
be extremely useful. For example, being able to tag photo albums as
work-related, and then giving conditional access to only the work related
photo albums in my media library to colleagues.
+1 elf Pavlik
0 csarven: Too specific. Complicated use case. [sw?]

### Conditional access by relationship
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-relationship

JUSTINWB: +1 - Cannot underscore how important this use case is to doing
intuitive authorization on more complex data objects. We cannot rely on
physical containment alone, because by definition solid is meant to connect
disparate data together into logical views that people can work with.
For example, if Alice wants to give Bob access to Project Y, and Project Y
is comprised of tasks, milestones, calendar events, contacts, and other
things stored in different places, how can she do that rationally? How can
she ensure that directive is maintained as new things are added? This use
case addresses that.
+1 elf Pavlik
-0 csarven: Way too specific. Prefer a general form of this UC eg. matching a data shape. [sw?]

### Conditional access by filter
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-filter

JUSTINWB: +0 - This is important, but I think we can hit it in the next cycle.
Ultimately, we need the manage access to data within the resource. There are
some rational ways to do this using machinery we already have.
+1 elf Pavlik
* +0 csarven: Too specific. May require querying. Alternative based on simpler use cases: hide references to resources from a container's index from agents that do not have read permission. [sw?]

### Conditional control boundaries
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-control

JUSTINWB: +0
+1 elf Pavlik
-0 csarven: Seems too convoluted for wide use. [sw?]

### Conditional access by action
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-action

JUSTINWB: 0
+1 elf Pavlik
+0 csarven: May be generally useful for [ap]. May [d].

## Permissioning Applications
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-applications

JUSTINWB: +1 - Limiting access to only specific applications, identified
by AppID, with the caveat that the effectiveness is specifically when
the user can strongly identify the application they are using (in
piloted scenarios)
0 csarven: it depends on the environment of the application.

## Privacy

### Limiting access to who else is permitted
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-whopermitted

JUSTINWB: +1 - Privacy in this context in paramount, unless the controller
specifically wants the information to be divulged.
+1 elf Pavlik
+1 csarven: Duplicate of #basic-readonly.

### Limiting access to other authorization conditions
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-historyofchanges

-0 csarven: Complex use case. Perhaps change focus to auxiliary resources.

### Minimal Credential Disclosure
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-minimalcredentials

JUSTINWB: +0 - This scenario is legitimate though I believe we'll likely
get to VC in the next cycle.
+1 elf Pavlik

### Limiting access to other authorization conditions
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-historyofchanges

JUSTINWB: +1 - Privacy in this context in paramount, unless the controller
specifically wants the information to be divulged.
* +1 csarven: Wide use. I don't plan to implement it.

### Limit information disclosure through URI
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-limituri

JUSTINWB: 0 - I'm not positive this is a use case for the authorization
system as much as how the resource server itself organizes and/or
represents data.
+1 elf Pavlik
+1 csarven: I don't plan to implement it.

## Trust

### Only trust certain issuers of identity
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-trustedissuers

JUSTINWB: +0 - Makes the authenticated agent use case more reasonable to me
+1 elf Pavlik
+1 csarven: Wide use. I don't plan to implement it.

## Validation
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-validation

JUSTINWB: +1 - This just seems like good engineering practice to me
0 csarven: Not sure why this UC is here or what's expected of it other than making sure server only processes valid authorization policies and in its absence all access is denied.

## Capabilities

### Possession of a verifiable credential
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-vc

JUSTINWB: +0 - Definitely seems to be a key part of the future of access
control for us.
+1 elf Pavlik
+1 csarven: Wide use.

### Possession of a link
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-link

JUSTINWB: +0 - Can be extremely beneficial in scenarios like invitation
flows or one-time shares.
+1 elf Pavlik
* 0 csarven: Some use but not essential (so, -1 wrt prioritisation)
