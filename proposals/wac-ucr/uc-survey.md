Add your name; add +1/+0/0/-0/-1 to a use case you find interesting and add a comment along these lines:

* +1 "yes, I need it, will implement, worth doing"
* +0 "I'm kind of for it, but can live without it"
* 0 "I really don't care about it"
* -0 "I'm kind of against it, but can live with it"
* -1 "I'm against this right now" (provide an explanation)

## Basic resource access

### Control access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-control

 - Jackson: +1

### Read-write access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-write

 - Jackson: +1

### Read-append access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend

#### Alice stores Danielle's recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-single-storage

- Jackson: +1

#### Danielle stores their own recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-multi-storage

 - Jackson: +1

### Append-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-appendonly

 - Jackson: +1

### Removing access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-removing

- Jackson: +1

### Read-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readonly

 - Jackson: +1

### Group access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-group

 - Jackson: +1

### Public access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-public

 - Jackson: +1

### Logged in access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-authenticated

 - Jackson: +1

## Basic collection access

### Read-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readonly

 - Jackson: +0 seems like a weak example, but it's needed for other examples

### Read-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readwrite

 - Jackson: -0 Some things need to be clarified here. If you have write access to a collection, do you also have write access to the resources of the collection? It wouldn't make sense for someone to be able to remove a resource from the collection if they don't have write access to that resource. Yet, on the other hand, if having read access on the collection doesn't give you read access to the resources, then it would be incongruent to allow write access to a collection to extend to the resource.

### Read-append access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappend

 - Jackson: +1

### Read-append-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappendwrite

 - Jackson: +1

### Append-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-appendonly

 - Jackson: +1

### Control access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-control

 - Jackson: +1

## Inheritance

### Read-only access to collection of resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readonly

 - Jackson: +1

### Read-append access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readappend

 - Jackson: 0 I think this needs more clarification on what happens to nested collections.

### Read-write access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readwrite

 - Jackson: 0 I think this needs more clarification on what happens to nested collections.

### Append-only access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-appendonly

 - Jackson: 0 I think this needs more clarification on what happens to nested collections.

### Control access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-control

 - Jackson: 0 I think this needs more clarification on what happens to nested collections.

### Default permissions on created resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-defaultcreated

 - Jackson: +1

### Default permissions for extended network
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-extended

 - Jackson: +1 Just change "extended network" to "a group" to clarify.

### Adding new subjects to inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-adding

 - Jackson: +1

### Modifying inherited permissions for existing subjects
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-modifying

 - Jackson: 0

### Forcing inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-forcing

 - Jackson: +1

## Conditional access

### Conditional access by time
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-time

 - Jackson: +1

### Conditional access by tag
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-tag

 - Jackson: 0

### Conditional access by relationship
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-relationship

 - Jackson: +1

### Conditional access by filter
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-filter

 - Jackson: +1

### Conditional control boundaries
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-control

 - Jackson: +1

### Conditional access by action
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-action

 - Jackson: 0

## Permissioning Applications
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-applications

 - Jackson: +1

## Privacy

### Limiting access to who else is permitted
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-whopermitted

 - Jackson: +1

### Limiting access to other authorization conditions
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-historyofchanges

 - Jackson: +1

### Minimal Credential Disclosure
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-minimalcredentials

 - Jackson: +1

### Limit information disclosure through URI
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-limituri

 - Jackson: +1

## Trust

### Only trust certain issuers of identity
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-trustedissuers

 - Jackson: +1

## Validation
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-validation

 - Jackson: +1

## Capabilities

### Possession of a verifiable credential
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-vc

 - Jackson: +1

### Possession of a link
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-link

 - Jackson: +1
