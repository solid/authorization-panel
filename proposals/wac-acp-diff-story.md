# ACP Compared to Web Access Control

This document is intended to reflect the following semi-authoritative (strongly suggestive) definitions:
- [W3C wiki](http://www.w3.org/wiki/WebAccessControl)
- [solid/specification](http://github.com/solid/specification)
- [sold/web-access-control](http://github.com/solid/solid-spec)
The goal is to ground conversations which compare WAC and ACP.
This is a work in progress.

Audience: those familiar with WAC or those who learn from comparisons

Access control is fundamentally about stating `who` can `do what` to `what resource`.

## ShEx for WAC:
Systems implementing Web Access Control (WAC) use a simple schema to make those assertions directly in RDF:

``` shex
PREFIX acl: <http://www.w3.org/ns/auth/acl#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

<AclShape> {
  # who
  (  acl:agentClass @<GroupShape> OR [foaf:Agent acl:AuthenticatedAgent] ;
    | acl:agentGroup @<GroupShape> OR IRI # likely in another resource
    | acl:agent IRI
    | acl:origin IRI
  )+ ;

  # can do what
  acl:mode [acl:Read acl:Write acl:Append acl:Control] + ;

  # to what resource
  (   acl:accessTo IRI
    | acl:accessToClass [foaf:Document] OR @<AclRegexShape> # TODO: remove Regexp, add auth user
  ) ;

  ^acl:trustedOrigin IRI ?
}

<GroupShape> { foaf:member IRI + }
<AclRegexShape> { acl:regex LITERAL }
```
([try it](http://shex.io/webapps/shex.js/doc/shex-simple?schema=PREFIX%20acl%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Fauth%2Facl%23%3E%0APREFIX%20foaf%3A%20%3Chttp%3A%2F%2Fxmlns.com%2Ffoaf%2F0.1%2F%3E%0A%0A%3CAclShape%3E%20%7B%0A%20%20%23%20who%0A%20%20(%20%20acl%3AagentClass%20%40%3CGroupShape%3E%20OR%20%5Bfoaf%3AAgent%20acl%3AAuthenticatedAgent%5D%20%3B%0A%20%20%20%20%7C%20acl%3Aagent%20IRI%0A%20%20%20%20%7C%20acl%3Aorigin%20IRI%0A%20%20)%2B%20%3B%0A%0A%20%20%23%20can%20do%20what%0A%20%20acl%3Amode%20%5Bacl%3ARead%20acl%3AWrite%20acl%3AAppend%20acl%3AControl%5D%20%2B%20%3B%0A%0A%20%20%23%20to%20what%0A%20%20(%20%20%20acl%3AaccessTo%20IRI%0A%20%20%20%20%7C%20acl%3AaccessToClass%20%5Bfoaf%3ADocument%5D%20OR%20%40%3CAclRegexShape%3E%0A%20%20)%20%3B%0A%0A%20%20%5Eacl%3AtrustedOrigin%20IRI%20%3F%0A%7D%0A%0A%3CGroupShape%3E%20%7B%20foaf%3Amember%20IRI%20%2B%20%7D%0A%0A%3CAclRegexShape%3E%20%7B%20acl%3Aregex%20LITERAL%20%7D%0A&data=PREFIX%20acl%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Fauth%2Facl%23%3E%0APREFIX%20foaf%3A%20%3Chttp%3A%2F%2Fxmlns.com%2Ffoaf%2F0.1%2F%3E%0A%0A%3Cacl1%3E%0A%20%20acl%3AaccessTo%20%3Ccard%3E%20%3B%0A%20%20acl%3Amode%20acl%3ARead%20%3B%0A%20%20acl%3AagentClass%20foaf%3AAgent%20.%0A%0A%3Cacl2%3E%0A%20%20acl%3AaccessTo%20%3Ccard%3E%20%3B%0A%20%20acl%3Amode%20acl%3ARead%2C%20acl%3AWrite%20%3B%0A%20%20acl%3Aagent%20%3Ccard%23i%3E.%0A%0A%3Cacl3%3E%0A%20%20acl%3AaccessTo%20%3Ccard%3E%20%3B%0A%20%20acl%3Amode%20acl%3ARead%20%3B%0A%20%20acl%3AagentClass%20%3Cfriends%23group%3E%20.%0A%0A%3Cacl4%3E%0A%20%20acl%3AaccessTo%20%3Ccard%3E%20%3B%0A%20%20acl%3Amode%20acl%3ARead%2C%20acl%3AWrite%20%3B%0A%20%20acl%3AagentClass%20%3Cfamily%23group%3E%20.%0A%0A%3Cfriends%23group%3E%20foaf%3Amember%20%3C..%2Fuser%2Falice%23me%3E%2C%20%3C..%2Fuser%2Fbob%23me%3E%2C%20%3C..%2Fuser%2Fcharlie%23me%3E%20.%20%0A%3Cfamily%23group%3E%20foaf%3Amember%20%20%3C..%2Fpeople%2Fdon%23me%3E%2C%20%3C..%2Fpeople%2Feloise%23me%3E%20.%0A%0A%3Cacl5%3E%0A%20%20acl%3AaccessToClass%20%5B%20acl%3Aregex%20%22https%3A%2F%2Fjoe.solid.example%2F.*%22%20%5D%20%3B%0A%20%20acl%3Amode%20acl%3ARead%20%3B%20%0A%20%20acl%3AagentClass%20foaf%3AAgent%20.%0A%0A%3Cacl6%3E%0A%20%20acl%3AaccessToClass%20%5B%20acl%3Aregex%20%22https%3A%2F%2Fbblfish.solid.example%2F.*%22%20%5D%20%3B%0A%20%20acl%3Amode%20acl%3AWrite%20%3B%0A%20%20acl%3Aorigin%20%3Chttps%3A%2F%2Fapps.rww.io%3E%20.%0A%0A%3C%23i%3E%20acl%3AtrustedOrigin%20%3Cacl7%3E%2C%20%3Cacl8%3E%20.%0A%3Cacl7%3E%0A%20%20acl%3Amode%20acl%3ARead%20%3B%0A%20%20acl%3AaccessToClass%20foaf%3ADocument%20%3B%20%20%23%3C-%20give%20access%20to%20all%20documents%20(%20that%20allow%20one%20access%20)%0A%20%20acl%3AagentClass%20foaf%3AAgent%20%3B%0A.%0A%3Cacl8%3E%0A%20%20acl%3Amode%20acl%3AWrite%20%3B%0A%20%20acl%3AaccessToClass%20foaf%3ADocument%20%3B%20%20%23%3C-%20give%20access%20to%20all%20documents%20(%20that%20allow%20access%20of%20course%20)%0A%20%20acl%3Aagent%20%3Chttps%3A%2F%2Fapps.w3.org%2F%3E%2C%20%3C%3E%20%23but%20only%20to%20JS%20agents%20that%20come%20from%20these%20two%20origins%0A.%0A&manifestURL=http%3A%2F%2Fshex.io%2Fwebapps%2Fshex.js%2Fexamples%2Fmanifest.json&shape-map=%7B%20FOCUS%20acl%3Amode%20_%20%7D%40%3CAclShape%3E&interface=minimal&success=proof&regexpEngine=eval-threaded-nerr))

For example, if Jezebel decides to let Bartholomew copy her physics assignment, she could write:

``` turtle
_:bart-copies-assignment-1
  acl:agent <http://solid.example/users/bart#id> ;
  acl:mode acl:Read ;
  acl:accessTo </courses/8.04/assignment-1> .
```
In Solid, this access document would appear in the same Container as `assignment-1`.

In principle, WAC lets her grant access to all assignments under `/courses/8.04/` &mdash;
``` turtle
_:bart-copies-my-assignments
  acl:agent <http://solid.example/users/bart#id> ;
  acl:mode acl:Read ;
  acl:accessToClass [ acl:regex ".*/courses/8.04/assignment-.*" ] .
```
&mdash; but Solid doesn't implement that feature, instead applying access to everything in a Container, e.g. &mdash;
``` turtle
_:bart-reads-8.04
  acl:agent <http://solid.example/users/bart#id> ;
  acl:mode acl:Read ;
  acl:accessTo </courses/8.04/> .
```


## ShEx for ACP

ACP maintains separate metadata associating resources to access control statements.

``` shex
PREFIX acp: <http://www.w3.org/ns/solid/acp#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

# who
<RuleShape> {
  a [acp:Rule] ? ;
  acp:agent IRI ;
}

# can do what
<AccessPolicyShape> {
  a [acp:AccessPolicy] ? ;
  acp:allow [acp:Write acp:Read acp:Append] + ;
  acp:allOf @<RuleShape> ;
}

# to what -- AccessControl doc linked to ACL'd resource with:
#   Link: <https://alice.pod/resume?ext=acp>; rel="http://www.w3.org/ns/solid/acp#accessControl"
<AccessControlShape> {
  a [acp:AccessControl] ? ;
  (
      acp:apply @<AccessPolicyShape>
    |
      ( acp:applyConstant @<AccessPolicyShape> ;
        acp:applyMembersConstant @<AccessPolicyShape> ;
      )
  )
}
```
([try it](http://shex.io/webapps/shex.js/doc/shex-simple?schema=PREFIX%20acp%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Fsolid%2Facp%23%3E%0APREFIX%20rdf%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0A%0A%23%20Policies%0A%0A%3CAccessPolicyShape%3E%20%7B%0A%20%20a%20%5Bacp%3AAccessPolicy%5D%20%3F%20%3B%0A%20%20acp%3Aallow%20%5Bacp%3AWrite%20acp%3ARead%5D%2B%20%3B%0A%20%20acp%3AallOf%20%40%3CRuleShape%3E%20%3B%0A%7D%0A%0A%3CRuleShape%3E%20%7B%0A%20%20a%20%5Bacp%3ARule%5D%20%3F%20%3B%0A%20%20acp%3Aagent%20IRI%20%3B%0A%7D%0A&data=PREFIX%20acp%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Fsolid%2Facp%23%3E%0APREFIX%20rdf%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0A%0A%3C%23i%3E%0A%20%20acp%3Aaccess%20%3C%23podControl%3E%20%3B%0A%20%20acp%3AaccessMembers%20%3C%23podControl%3E%20.%0A%3C%23myPodAccess%3E%0A%20%20a%20acp%3AAccessControl%20%3B%0A%20%20acp%3AapplyConstant%20%3C%23podControl%3E%20%3B%0A%20%20acp%3AapplyMembersConstant%20%3C%23podControl%3E%20.%0A%3C%23resumeAssistance%3E%0A%20%20a%20acp%3AAccessControl%20%3B%0A%20%20acp%3Aapply%20%3C%23personalTrusted%3E%20.%0A%0A%23%20Policies%0A%0A%3C%23personalTrusted%3E%0A%20%20a%20acp%3AAccessPolicy%20%3B%0A%20%20acp%3Aallow%20acp%3AWrite%2C%20acp%3ARead%20%3B%0A%20%20acp%3AallOf%20%3C%23editorFriends%3E%20.%0A%0A%3C%23podControl%3E%0A%20%20a%20acp%3AAccessPolicy%20%3B%0A%20%20acp%3Aallow%20acp%3AWrite%2C%20acp%3ARead%20%3B%0A%20%20acp%3AallOf%20%3C%23accessControllers%3E%20.%0A%0A%23%20Rules%0A%0A%3C%23editorFriends%3E%0A%20%20a%20acp%3ARule%20%3B%0A%20%20acp%3Aagent%20%3Chttps%3A%2F%2Fbob.pod%2Fprofile%2Fcard%23me%3E%20.%0A%0A%3C%23accessControllers%3E%0A%20%20a%20acp%3ARule%20%3B%0A%20%20acp%3Aagent%20%3Chttps%3A%2F%2Falice.pod%2Fprofile%2Fcard%23me%3E%20.%0A&manifestURL=http%3A%2F%2Fshex.io%2Fwebapps%2Fshex.js%2Fexamples%2Fmanifest.json&shape-map=%7BFOCUS%20acp%3Aallow%20_%7D%40%3CAccessPolicyShape%3E&interface=human&success=proof&regexpEngine=eval-threaded-nerr))

As above, Jezebel can let Bartholomew copy her homework by editing the ACL document associated with `</courses/8.04/assignment-1>` by the `acp:accessControl` link header (e.g., `</courses/8.04/assignment-1?access>`):

``` turtle
_:bart-copies-assignment-1
  a acl:AccessControl ;
  acp:apply [
    a acp:AccessPolicy ;
    acp:allow acp:Read ;
    acp:allOf [
      acp:agent <http://solid.example/users/bart#id>
    ]
  ] .
```

She could instead move that policy someplace independent from the existence of `</courses/8.04/assignment-1>`, e.g., `</myAccessPolicies>`, so it won't disappear if she deletes or renames `assignment-1`:
``` turtle
<#bart-copies-my-homework>
  a acp:AccessPolicy ;
  acp:allow acp:Read ;
  acp:allOf [
    acp:agent <http://solid.example/users/bart#id>
  ] .
```
Now she can apply it to multiple homework documents or all of her 8.04 documents with individual access controls like:
``` turtle
_:bart-copies-assignment-2
  a acl:AccessControl ;
  acp:apply </myAccessPolicies#bart-copies-my-homework> .
```
