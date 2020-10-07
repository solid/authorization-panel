Add your name; add +1/+0/0/-0/-1 to a use case you find interesting and add a comment along these lines:

* +1 "yes, I need it, will implement, worth doing"
* +0 "I'm kind of for it, but can live without it"
* 0 "I really don't care about it"
* -0 "I'm kind of against it, but can live with it"
* -1 "I'm against this right now" (provide an explanation)

## Basic resource access

### Control access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-control

JUSTINWB: +1

### Read-write access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-write

JUSTINWB: +1

### Read-append access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend

JUSTINWB: +1

#### Alice stores Danielle's recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-single-storage

JUSTINWB: +1

#### Danielle stores their own recommendation
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readappend-multi-storage

JUSTINWB: +1

### Append-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-appendonly

JUSTINWB: +1

### Removing access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-removing

JUSTINWB: +1

### Read-only access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-readonly

JUSTINWB: +1

### Group access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-group

JUSTINWB: +1

### Public access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-public

JUSTINWB +1

### Logged in access
URL: https://solid.github.io/authorization-panel/wac-ucr/#basic-authenticated

JUSTINWB +0 - There's controversy over whether this can be valuable given
the ability for anyone to spin up their own identity in a decentralized
system. I think that in concert with the ability to specify or verify
trusted issuers this can be valuable and probably beneficial in a number of
scenarios.

## Basic collection access

### Read-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readonly

JUSTINWB: +1

### Read-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readwrite

JUSTINWB: +1

### Read-append access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappend

JUSTINWB: +1

### Read-append-write access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-readappendwrite

JUSTINWB: +1 - Being able to designate the creator of a resource with specific
privileges in an append scenario on a container is extremely important to
a number of collaborative scenarios.

### Append-only access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-appendonly

JUSTINWB: +1

### Control access to a Collection
URL: https://solid.github.io/authorization-panel/wac-ucr/#collection-control

JUSTINWB: +1

## Inheritance

### Read-only access to collection of resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readonly

JUSTINWB: +1

### Read-append access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readappend

JUSTINWB: +1

### Read-write access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-readwrite

JUSTINWB: +1

### Append-only access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-appendonly

JUSTINWB: +1

### Control access to collection resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-control

JUSTINWB: +1

### Default permissions on created resources
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-defaultcreated

JUSTINWB: +1

### Default permissions for extended network
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-extended

JUSTINWB: +1

### Adding new subjects to inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-adding

JUSTINWB: +1 - Without an extensible inheritance system, it is near impossible
to do any permission management that doesn't require managing permissions
for every resource specifically, lest they negate permissions that were set
above.

### Modifying inherited permissions for existing subjects
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-modifying

JUSTINWB: +1 - Without an extensible inheritance system, it is near impossible
to do any permission management that doesn't require managing permissions
for every resource specifically, lest they negate permissions that were set
above.

### Forcing inherited permissions
URL: https://solid.github.io/authorization-panel/wac-ucr/#inheritance-forcing

JUSTINWB: +1 - Some permissions shouldn't be contradicted. For example,
the administrator with full control access of a given storage (i.e. pod)
shouldn't be able to have their permissions denied by someone with control
access to a collection far down in the hierarchy so they can hide things
inside.

## Conditional access

### Conditional access by time
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-time

JUSTINWB: +1 - Setting timeouts can really come in handy, and is a good
way to ensure that permissions meant to be short-lived don't hang around
longer than they're needed. Also very useful when used in concert with
invitiation flows.

### Conditional access by tag
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-tag

JUSTINWB: +1 - Noting that we haven't specified a tag-based system for data
yet in solid, the ability to tag individual resources, or collections of
resources, and then have conditions in ACLs based on those tags, can
be extremely useful. For example, being able to tag photo albums as
work-related, and then giving conditional access to only the work related
photo albums in my media library to colleagues.

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

### Conditional access by filter
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-filter

JUSTINWB: +0 - This is important, but I think we can hit it in the next cycle.
Ultimately, we need the manage access to data within the resource. There are
some rational ways to do this using machinery we already have.

### Conditional control boundaries
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-control

JUSTINWB: +0

### Conditional access by action
URL: https://solid.github.io/authorization-panel/wac-ucr/#conditional-action

JUSTINWB: 0

## Permissioning Applications
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-applications

JUSTINWB: +1 - Limiting access to only specific applications, identified
by AppID, with the caveat that the effectiveness is specifically when
the user can strongly identify the application they are using (in
piloted scenarios)

## Privacy

### Limiting access to who else is permitted
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-whopermitted

JUSTINWB: +1 - Privacy in this context in paramount, unless the controller
specifically wants the information to be divulged.

### Limiting access to other authorization conditions
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-historyofchanges

JUSTINWB: +1 - Privacy in this context in paramount, unless the controller
specifically wants the information to be divulged.

### Minimal Credential Disclosure
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-minimalcredentials

JUSTINWB: +0 - This scenario is legitimate though I believe we'll likely
get to VC in the next cycle.

### Limit information disclosure through URI
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-limituri

JUSTINWB: 0 - I'm not positive this is a use case for the authorization
system as much as how the resource server itself organizes and/or
represents data.

## Trust

### Only trust certain issuers of identity
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-trustedissuers

JUSTINWB: +0 - Makes the authenticated agent use case more reasonable to me

## Validation
URL: https://solid.github.io/authorization-panel/wac-ucr/#uc-validation

JUSTINWB: +1 - This just seems like good engineering practice to me

## Capabilities

### Possession of a verifiable credential
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-vc

JUSTINWB: +0 - Definitely seems to be a key part of the future of access
control for us.

### Possession of a link
URL: https://solid.github.io/authorization-panel/wac-ucr/#capabilities-link

JUSTINWB: +0 - Can be extremely beneficial in scenarios like invitation
flows or one-time shares.
