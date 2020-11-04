# 3. Definitions

## Agent

*   Any entity (person, organization, device, etc.) identified by a [WebID](#webid).

## Group

*   A Group is identified by an [IRI](https://en.wikipedia.org/wiki/Internationalized_Resource_Identifier).
*   A Group has a set of members where each member can be an [agent](#agent) or another group. 

![alt_text](diagrams/group.svg "image_tooltip")

## WebID

*   A [WebID](https://dvcs.w3.org/hg/WebID/raw-file/tip/spec/identity-respec.html) is an HTTP URI that identifies an [Agent](#agent). 

## Access Control Resource

*   An Access Control Resource (ACR) is a resource containing access control metadata associated with a resource R.
*   Every resource MUST have an ACR.
*   The ACR MUST be in the same Pod as the resource R.

![alt_text](diagrams/resource-acr.svg "image_tooltip")

*   An ACR consists of zero or more [Access Controls](#access-control) that determine the access to the resource linked to the ACR.
*   An ACR uses three pairs of predicates to specify the [Access Policies](#access-policy) used to determine the agents that can Read or Write the ACR. The Append access mode is ignored by these predicates.

    **1. Adding and removing normal policies**

    *   The _acp:access_ predicate specifies the [Access Policies](#access-policy) that determine the agents that can add policies to and remove policies from the ACR.
    *   The _acp:accessMembers_ predicate specifies the [Access Policies](#access-policy) that are propagated to the _acp:access_ and _acp:accessMembers_ predicates for child resources (see [Access Control](#access-control)) to the resource associated with this ACR.

    **2. Adding and removing protected policies**

    *   The _acp:accessProtected_ predicate specifies the [Access Policies](#access-policy) that determine the agents that can add protected policies to and remove protected policies from the ACR.
    *   The _acp:accessMembersProtected_ predicate specifies the  [Access Policies](#access-policy) that are propagated to the _acp:accessProtected_ and _acp:accessMembersProtected_ predicates for child resources (see [Access Control](#access-control)) to the resource associated with this ACR.

    **3. Adding and removing locked policies**

    *   The _acp:accessLocked_ predicate specifies the [Access Policies](#access-policy) that determine the agents that can add locked policies to and remove locked policies from the ACR.
    *   The _acp:accessMembersLocked_ predicate specifies the  [Access Policies](#access-policy) that are propagated to the _acp:accessLocked_ and _acp:accessMembersLocked_ predicates for child resources (see [Access Control](#access-control)) to the resource associated with this ACR.
*   The ACR for the root container has an _acp:accessPodOwner_ predicate whose object is the [Access Policies](#access-policy) used to determine the agents that will receive the http://www.w3.org/ns/solid/terms:PodOwner link header. If the _acp:accessPodOwner_ predicate is not provided then the link header will be included in responses by default.

![alt_text](diagrams/acr-policies.svg "image_tooltip")


*   The ACR MAY contain [Access Policies](#access-policy) (AP) and [Access Rules](#access-rule) (AR). This allows an [Agent](#agent) with _acp:Write_ access to an ACR to specify a policy giving them access to the Resource controlled by the ACR.  Given the Pod Owner always has _acp:Write_ access to all ACRs, they can always give themselves write access to any resource if they wish. This is also the mechanism that can be used to bootstrap access to a Pod by the [Pod Owner](#pod-owner) if no policies exist in the Pod; they can create a simple policy giving themselves write access to the root Container.


## Access Control

*   An Access Control (AC) statement specifies the [Access Policies](#access-policy) (AP) used to determine the [Access Modes](#access-mode) an agent gets to the resource R linked to the [ACR](#access-control-resource).
*   All AP MUST be in the same Pod as the resource R. 
*   An AC can use the following  predicates to determine access to the resource (R) linked to the [ACR](#access-control-resource).
    *   _acp:apply_: Specifies zero or more [Access Policies](#access-policy) that determine access to R.
    *   _acp:applyProtected_: (Post GA) Specifies zero or more [Access Policies](#access-policy) that determine access to R. These [Access Policies](#access-policy) can only be removed from the ACR by an [agent](#agent) who has acp:Write access to the ACR where the AP was originally added. If an AP is added directly to an ACR then _acp:apply_ and _acp:applyProtected_ have the same semantics. However if an AP is propagated from an ACR higher in the tree then an [Agent](#agent) could only remove the AP if they had acp:Write access to the ACR higher in the tree where the AP was originally added and subsequently propagated from.  
    *   _acp:applyLocked_: (Post GA) Specifies zero or more [Access Policies](#access-policy) that determine access to R. These [Access Policies](#access-policy) can only be removed from the ACR by an [agent](#agent) who has acp:Write access to the root ‘/’ of the Pod.
    *   acp:applyMembers: Specifies zero or more [Access Policies](#access-policy) that are propagated to the [ACR](#access-control-resource) of the children of R as the _acp:apply_ and acp:applyMembers predicates when the children are created or when an [Access Policy](#access-control) is added to the ACR.
    *   _acp:applyMembersProtected_:(Post GA) Specifies zero or more [Access Policies](#access-policy) that are propagated to the [ACR](#access-control-resource) of the children of R as the _acp:applyProtected_ and acp:applyMembersProtected predicates when the children are created or when an [Access Policy](#access-control) is added to the ACR. These [Access Policies](#access-policy) can only be removed from the ACR  by an [agent](#agent) who has acp:Write access to the ACR where the AP was originally added.
    *   _acp:applyMembersLocked_:(Post GA) Specifies zero or more [Access Policies](#access-policy) that are propagated to the [ACR](#access-control-resource) of the children of R as the _acp:applyLocked_ and acp:applyMembersLocked predicates when the children are created or when an [Access Policy](#access-control) is added to the ACR. These [Access Policies](#access-policy) can only be removed from the ACR by an agent who has acp:Write access to the root ‘/’ of the Pod.

![alt_text](diagrams/ac-policies.svg "image_tooltip")

## Access Policy Resource

*   An Access Policy Resource (APR) is a resource containing metadata describing the [Access Policies](#access-policy) and [Access Rules](#access-rule) that can be used to determine agent (or group) access to resources. Any resource can act as an Access Policy Resource.
*   An APR contains zero or more [Access Policies](#access-policy) and zero or more [Access Rules](#access-rule) used by the [Access Policies](#access-policy).
*   [Access Policies](#access-policy) and [Access Rules](#access-rule) can also be defined in [Access Control Resources](#access-control-resource), but defining them in an APR means they are reusable by agents and manageable by agents without those agents needing access to ACR metadata. 

![alt_text](diagrams/apr.svg "image_tooltip")

## Access Policy

*   An Access Policy (AP) specifies a list of [rules](#access-rule) to be satisfied when determining whether an [agent](#agent) has access to a resource. 
*   An _acp:deny_ predicate is used to specify the [Access Modes](#access-mode) that are denied if the [Access Rules](#access-rule) are satisfied.
*   An _acp:allow_ predicate is used to specify the [Access Modes](#access-mode) that are allowed if the [Access Rules](#access-rule) are satisfied.
*   The _acp:allOf_ predicate specifies a set of zero or more [Access Rules](#access-rule). All of the rules must be satisfied for the AP to be satisfied.
*   The _acp:anyOf_ predicate specifies a set of zero or more [Access Rules](#access-rule). At least one of the rules must be satisfied for the AP to be satisfied.
*   The _acp:noneOf_ predicate specifies a set of zero or more [Access Rules](#access-rule). None of the rules can be satisfied if that AP is to be satisfied.

![alt_text](diagrams/ap.svg "image_tooltip")

## Access Rule

*   An Access Rule (AR) specifies conditions that must be true for the AR to be satisfied.
*   At present the AR only allows the [agent](#agent) trying to access a resource to be matched. In future AR can provide the ability to apply more sophisticated conditions. Examples include time constraints and use of verifiable credentials.
*   2 predicates can be used to match an [agent](#agent):
    *   The _acp:agent _predicate can specify:
        *   zero or more [WebIDs](#webid).
        *   _acp:PublicAgent_.
        *   _acp:CreatorAgent_ to mean the agent who created the resource.
        *   _acp:AuthenticatedAgent_ to mean any agent who has authenticated.
    *   The _acp:group_ predicate can specify zero or more [groups](#group) and matches if the agent is a member of any of the groups. 

![alt_text](diagrams/ar.svg "image_tooltip")

## Access Mode

Access Modes describe a type of access to resources.

<table>
  <tr>
   <td><strong>AccessMode</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>Read
   </td>
   <td>View the data
   </td>
  </tr>
  <tr>
   <td>Write
   </td>
   <td>Write and delete the data
   </td>
  </tr>
  <tr>
   <td>Append
   </td>
   <td>Add data
   </td>
  </tr>
</table>

## Pod Owner 

*   Each Pod provisioned by a Solid Server MUST have exactly one Pod Owner.
*   The Pod Owner MUST be a [WebID](#webid).
*   The Pod Owner metadata MUST be managed by the Solid Server.
*   The Pod Owner MUST always have Read and Write access to all [Access Control Resources](#access-control-resource) (ACR) i.e. it is not possible to remove Read or Write access from a Pod Owner from ACRs.
*   The Pod Owner WebID is returned as a Link header in response to a GET or HEAD request on a resource 
    *   Link: &lt;[https://bob.pod/profile/card#me](https://bob.pod/profile/card#me)>; rel="http://www.w3.org/ns/solid/acp#PodOwner"
*   The acp:PodOwner link header is returned if the [Agent](#agent) has acp:Read access to the Resource and:
    *   the _acp:accessPodOwner_ predicate does not exist in the root [Access Control Resource](#access-control-resource) or
    *   the _acp:accessPodOwner_ predicate exists in the root [Access Control Resource](#access-control-resource) and provides the [Agent](#agent) with Read access.
*   A Solid Server MUST allow Pod Ownership to be transferred to a different [agent](#agent) (Post GA)
