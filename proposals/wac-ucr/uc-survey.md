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

* +1 csarven: Wide use. Any agent having the ability to change access controls has wide use. Required for [ap]. [d].

### Read-write access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-write

* +1 csarven: Wide use. Required for [ap]. [d].

### Read-append access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend

Notes:
* csarven: The resume resource is assumed to be simple ie. a single unit, and there is no assumption about it being a dynamic or compound resource. UC #basic-readappend assumes an append operation that can add information to a specific part of a resource ie. the references section. An agent can append information to a resource (as in #basic-appendonly) but there is no guarantee or validation assumed to take place as part of the use case - hence, other requirements needs to be met in parallel for this use case to actually work.

#### Alice stores Danielle's recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-single-storage

* +1 csarven: Generally worth doing. This UC implies a requirement that ensures data integrity.

#### Danielle stores their own recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-multi-storage

* +1 csarven: [d] such that agent A sends a notification about the recommendation to agent B's inbox ie. #collection-readappend , instead of updating a resource that references it.

### Append-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-appendonly

* +1 csarven: Convoluted scenario but the use case has wide use. [d] with some variations.

### Removing access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-removing

* +1: csarven: Wide use. This UC is possibly a duplicate of #basic-control. [d].

### Read-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readonly

* +1 csarven: Wide use. Required for [ap]. [d].

### Group access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-group

* +1 csarven: This should be core but can live without it. May [d]. Group management may not be common for [ap].

### Public access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-public

* +1 csarven: Wide use. Required for [ap]. [d].

### Logged in access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-authenticated

* +1 csarven: Wide use. Required for [ap]. [d].

## Basic collection access

### Read-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readonly

* +1 csarven: Wide use. Required for [ap]. [d].

### Read-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readwrite

* +1 csarven: Wide use. Required for [ap]. [d].

### Read-append access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappend

* +1 csarven: Wide use. Required for [ap]. [d].

### Read-append-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappendwrite

* 0 csarven: Seems like duplicate of #collection-readwrite and #collection-readappend

### Append-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-appendonly

* +1 csarven: Wide use. Required for [ap] eg. creating annotations or notifications. [d].

### Control access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-control

* +1 csarven: Wide use. Any agent having the ability to change access controls has wide use. Required for [ap]. [d].

## Inheritance

### Read-only access to collection of resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readonly

* +1 csarven: Wide use. Required for [ap]. [d].

### Read-append access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readappend

* +1 csarven: Wide use. Required for [ap] eg. need to set base access to read and share annotations and notifications. [d].

### Read-write access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readwrite

* +1 csarven: Wide use. Required for [ap] - similar to #inheritance-readappend. [d].

### Append-only access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-appendonly

* +1 csarven: Wide use. Required for [ap] ie. to be able to update existing resources or to create new ones in a container. [d].

### Control access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-control

* +1 csarven: Wide use. Required for [ap]. [d].

### Default permissions on created resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-defaultcreated

* +0 csarven: Difference between default and inherited is not particularly significant for [ap]  - one is sufficient for most common use [d].

### Default permissions for extended network
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-extended

* +0 csarven: Not core but probably necessary for some application domains.

### Adding new subjects to inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-adding

* +1 csarven: May be duplicating simpler use cases. May [d].

### Modifying inherited permissions for existing subjects
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-modifying

* +1 csarven: May be duplicating simpler use cases. May [d].

### Forcing inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-forcing

* +0 csarven: Could be useful for [ap]. May be implemented in [d] depending on other available features. See also note on #collection-control.

## Conditional access

### Conditional access by time
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-time

* +0 csarven:

### Conditional access by tag
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-tag

* 0 csarven: Too specific. Complicated use case. [sw?]

### Conditional access by relationship
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-relationship

* -0 csarven: Way too specific. Prefer a general form of this UC eg. matching a data shape. [sw?]

### Conditional access by filter
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-filter

* +0 csarven: Too specific. May require querying. Alternative based on simpler use cases: hide references to resources from a container's index from agents that do not have read permission. [sw?]

### Conditional control boundaries
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-control

* -0 csarven: Seems too convoluted for wide use. [sw?]

### Conditional access by action
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-action

* +0 csarven: May be generally useful for [ap]. May [d].

## Permissioning Applications
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-applications

* 0 csarven: it depends on the environment of the application.

## Privacy

### Limiting access to who else is permitted
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-whopermitted

* +1 csarven: Duplicate of #basic-readonly.

### Limiting access to other authorization conditions
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-historyofchanges

* -0 csarven: Complex use case. Perhaps change focus to auxiliary resources.

### Minimal Credential Disclosure
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-minimalcredentials

* +1 csarven: Wide use. I don't plan to implement it.

### Limit information disclosure through URI
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-limituri

* +1 csarven: I don't plan to implement it.

## Trust

### Only trust certain issuers of identity
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-trustedissuers

* +1 csarven: Wide use. I don't plan to implement it.

## Validation
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-validation

* 0 csarven: Not sure why this UC is here or what's expected of it other than making sure server only processes valid authorization policies and in its absence all access is denied.

## Capabilities

### Possession of a verifiable credential
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-vc

* +1 csarven: Wide use.

### Possession of a link
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-link

* 0 csarven: Some use but not essential (so, -1 wrt prioritisation)
