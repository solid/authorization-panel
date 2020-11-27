# 4. Enforcement & Propagation

## Access Control Enforcement

### HTTP Method to Access Mode

The table below maps HTTP methods to the required _acp:AccessMode_.

<table>
  <tr>
   <td>Method
   </td>
   <td>Context
   </td>
   <td>Access Mode Required
   </td>
  </tr>
  <tr>
   <td>GET
   </td>
   <td>
   </td>
   <td>Read
   </td>
  </tr>
  <tr>
   <td>PUT
   </td>
   <td>Resource does not exist
   </td>
   <td>Append
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Resource already exists
   </td>
   <td>Write
   </td>
  </tr>
  <tr>
   <td>POST
   </td>
   <td>
   </td>
   <td>Append
   </td>
  </tr>
  <tr>
   <td>PATCH
   </td>
   <td>With deletes
   </td>
   <td>Write
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Without deletes
   </td>
   <td>Append
   </td>
  </tr>
  <tr>
   <td>DELETE
   </td>
   <td>
   </td>
   <td>Write
   </td>
  </tr>
  <tr>
   <td>HEAD
   </td>
   <td>
   </td>
   <td>Read
   </td>
  </tr>
</table>

### Access Enforcement

When an [Agent](definitions.md#agent) A attempts to access a Resource R using HTTP method M, the following algorithm is used to enforce access control.

[Access Enforcement](access-enforcement.md)

## Policy Propagation

[Policy Propagation](policy-propagation.md)

## Access Control Visibility

### Visibility of access to a resource

The response to a HEAD or GET  request from an [Agent](definitions.md#agent) on a resource will include link headers where the relation type is [http://www.w3.org/ns/solid/acp#allow](http://www.w3.org/ns/solid/acp#allow), that indicates the level of access the [Agent](definitions.md#agent) has to the resource. 


<table>
  <tr>
   <td><strong>Link</strong>
   </td>
   <td><strong>Access Permitted</strong>
   </td>
  </tr>
  <tr>
   <td><a href="http://www.w3.org/ns/solid/acp#Read">http://www.w3.org/ns/solid/acp#Read</a>
   </td>
   <td>Read access.
   </td>
  </tr>
  <tr>
   <td colspan="2" >Link : &lt;<a href="http://www.w3.org/ns/solid/acp#Read">http://www.w3.org/ns/solid/acp#Read</a>>; rel=”<a href="http://www.w3.org/ns/solid/acp#allow">http://www.w3.org/ns/solid/acp#allow</a>”
   </td>
  </tr>
  <tr>
   <td><a href="http://www.w3.org/ns/solid/acp#Append">http://www.w3.org/ns/solid/acp#Append</a>
   </td>
   <td>Append access. 
   </td>
  </tr>
  <tr>
   <td colspan="2" >Link : &lt;<a href="http://www.w3.org/ns/solid/acp#Append">http://www.w3.org/ns/solid/acp#Append</a>>; rel=”<a href="http://www.w3.org/ns/solid/acp#allow">http://www.w3.org/ns/solid/acp#allow</a>”
   </td>
  </tr>
  <tr>
   <td><a href="http://www/w3.org/ns/solid/acp#Write">http://www/w3.org/ns/solid/acp#Write</a>
   </td>
   <td>Write access.
   </td>
  </tr>
  <tr>
   <td colspan="2" >Link : &lt;<a href="http://www.w3.org/ns/solid/acp#Write">http://www.w3.org/ns/solid/acp#Write</a>>; rel=”<a href="http://www.w3.org/ns/solid/acp#allow">http://www.w3.org/ns/solid/acp#allow</a>”
   </td>
  </tr>
</table>

Note that the links may be provided on one line as follows.

```HTTP
Link : <http://www.w3.org/ns/solid/acp#Read> rel=http://www/w3.org/ns/solid/acp#allow",
       <http://www.w3.org/ns/solid/acp#Append> rel=http://www/w3.org/ns/solid/acp#allow",
       <http://www.w3.org/ns/solid/acp#Write> rel=http://www/w3.org/ns/solid/acp#allow"
```

If the [Agent](definitions.md#agent) does not have  [http://www.w3.org/ns/solid/acp#Read](http://www.w3.org/ns/solid/acp#Read) access to the Resource, then the response to a GET or HEAD request will be a HTTP status code 403 but the headers described above will still be provided. The agent may have [http://www.w3.org/ns/solid/acp#Write](http://www.w3.org/ns/solid/acp#Write) or [http://www.w3.org/ns/solid/acp#Append](http://www.w3.org/ns/solid/acp#Append) access.

Note that for both a HEAD request and a GET request the server may be configured to respond with a HTTP status code 404 if the existence of the resource should not be revealed to a client. 

### Visibility of access to Access Control Resource

An Agent has Read access to the ACR for a resource if they have Read access according to the policies specified by any of the following predicates:

*   _acp:access_

An Agent has Write access to the ACR for a resource if they have Write access according to the policies specified by any of the following predicates:

*   _acp:access_
    * Policies can be added using _acp:apply_ and _acp:applyMembers_
    * Statements using _acp:apply_ and _acp:applyMembers_ can be removed
*   _acp:accessProtected_
    * Policies can be added using _acp:applyProtected_ and _acp:applyMembersProtected_
    * Statements using _acp:applyProtected_ and _acp:applyMembersProtected_ can only be removed if an agent has write access via _acp:access_ to the ACR in which the statement was originally added. For example, if the statement was originally added to a/b/ and later propagated to a/b/c/d then the statement can only be removed from a/b/c/d if the agent has write access to the ACR for a/b/.
*   _acp:accessLocked_
    * Policies can be added using _acp:applyLocked_ and _acp:applyMembersLocked_
    * Statements using _acp:applyLocked_ and _acp:applyMembersLocked_ can only be removed if an agent has write access via _acp:access_ to the ACR for the root '/' container. 

Note that the Append mode is not relevant for access to [Access Control Resources](definitions.md#access-control-resource). 

A successful GET or HEAD request on an ACR will contain the following link header

```HTTP
Link : <http://www.w3.org/ns/solid/acp#AccessControlResource>; rel="type"
```

## ACR Access Control Enforcement


### HTTP Method to ACR Access Mode

The table below maps HTTP methods to the required _acp:AccessMode_ when handling ACR metadata.

<table>
  <tr>
   <td>Method
   </td>
   <td>Context
   </td>
   <td>Access Mode Required
   </td>
  </tr>
  <tr>
   <td>GET
   </td>
   <td>
   </td>
   <td>Read
   </td>
  </tr>
  <tr>
   <td>PUT
   </td>
   <td>
   </td>
   <td>Write
   </td>
  </tr>
  <tr>
   <td>POST
   </td>
   <td>Not allowed
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>PATCH
   </td>
   <td>
   </td>
   <td>Write
   </td>
  </tr>
  <tr>
   <td>DELETE
   </td>
   <td>Not allowed
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>HEAD
   </td>
   <td>
   </td>
   <td>Read or Write
   </td>
  </tr>
</table>

When an [Agent](definitions.md#agent) A attempts to access an [Access Control Resource](definitions.md#access-control-resource) ACR using HTTP method M, the following algorithm is used to enforce access control.

[ACR access enforcement](acr-access-enforcement.md)

## ACR Policy Propagation 

[ACR policy propagation](acr-policy-propagation.md)
