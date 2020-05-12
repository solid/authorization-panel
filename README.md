
[![Join the chat at https://gitter.im/solid/app-authorization-panel](https://badges.gitter.im/solid/app-authorization-panel.svg)](https://gitter.im/solid/app-authorization-panel?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Solid Authorization and Access Control Panel.

## Meetings

Meetings of the Authorization and Access Control panel take place at 10AM eastern on Mondays. Meeting details listed below:

```
https://inrupt.my.webex.com/inrupt.my/j.php?MTID=m131f1588eb27afc298276f1cbf077a79
Meeting number:
792 295 998
Password: a9pRfXbZe78
More ways to join
Join by phone
+1-408-418-9388 United States Toll
Access code: 792 295 998
```

## Success Criteria:

The aim of this is to produce a candidate proposal to the [Solid Specification](https://github.com/solid/specification) that defines a mechanism to allow apps to communicate to users what they want to access while giving the users the power to dictate access easily.

A successful spec will satisfy the following requirements:
 - The system is not abusable
 - An app can request access to a specific resource
 - An app can request access to a specific type of data without knowing the structure of resources on a Pod
 - Access requests can be sent when the resource owner is not present to be approved once the user is present
 - Apps can request the ability to write a specific type of data and will be told where it should write it
 - It should be possible for an agent to block/allow certain apps from accessing a specific resource as that agent
 - It should be possible for an agent with Control access to block/allow certain apps from accessing a specific resource as any agent
 - It should be easy to allow others accessing your resources to use apps you're okay without requiring your explicit consent.
 - Access to specific types of data should extend to new resources that contain that data
 - Access to specific types of data should not expose other data that was not requested
 - Data should have different levels of requirements for user's conciousness in consent
 - The authorization data is easily cachable
 - Information about Clients (apps) to which users have granted access and what specific access they have delegated should not be made available to Resource Servers that do not enforce relevant access restrictions on those Clients.
