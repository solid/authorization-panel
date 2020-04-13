
[![Join the chat at https://gitter.im/solid/app-authorization-panel](https://badges.gitter.im/solid/app-authorization-panel.svg)](https://gitter.im/solid/app-authorization-panel?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Solid Authorization and Access Control Panel.

## Meetings

Meetings of the Authorization and Access Control panel take place at 10AM eastern on Mondays. Meeting details listed below:

Jackson Morgan is inviting you to a scheduled Zoom meeting.

Topic: Solid Authentication Spec Meeting

Time: Nov 25, 2019 10:00 AM Eastern Time (US and Canada)

* Apr 6, 2020 10:00 AM
* Apr 13, 2020 10:00 AM
* Apr 20, 2020 10:00 AM
* Apr 27, 2020 10:00 AM
* May 4, 2020 10:00 AM
* May 11, 2020 10:00 AM
* May 18, 2020 10:00 AM
* May 25, 2020 10:00 AM
* Jun 1, 2020 10:00 AM

Please download and import the following iCalendar (.ics) files to your calendar system.

Weekly: https://zoom.us/meeting/uZMsfumtrT4ienG4YVCoaXYr3ymOmaRuvQ/ics?icsToken=98tyKu2vrzIqH9eQuFztf7UvA4H5bM-1i3RAjfdkqjzqAhhqcFDvO81PM4BqBumB

Join Zoom Meeting
https://zoom.us/j/471905358

Meeting ID: 471 905 358

One tap mobile
+16465588656,,471905358# US (New York)
+17207072699,,471905358# US (Denver)

Dial by your location
* +1 646 558 8656 US (New York)
* +1 720 707 2699 US (Denver)
Meeting ID: 471 905 358
Find your local number: https://zoom.us/u/aZFsbOjDu

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
