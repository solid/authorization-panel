
[![Join the chat at https://gitter.im/solid/app-authorization-panel](https://badges.gitter.im/solid/app-authorization-panel.svg)](https://gitter.im/solid/app-authorization-panel?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Solid Authorization and Access Control Panel.

## Meetings

Meetings are every Wednesday at 10AM eastern. Details of the meeting are posted below:

Jackson Morgan is inviting you to a scheduled Zoom meeting.

Topic: Solid Authorization Spec Meeting
Time: Nov 27, 2019 10:00 AM Eastern Time (US and Canada)
        Every week on Wed, until Jun 3, 2020, 28 occurrence(s)
        Nov 27, 2019 10:00 AM
        Dec 4, 2019 10:00 AM
        Dec 11, 2019 10:00 AM
        Dec 18, 2019 10:00 AM
        Dec 25, 2019 10:00 AM
        Jan 1, 2020 10:00 AM
        Jan 8, 2020 10:00 AM
        Jan 15, 2020 10:00 AM
        Jan 22, 2020 10:00 AM
        Jan 29, 2020 10:00 AM
        Feb 5, 2020 10:00 AM
        Feb 12, 2020 10:00 AM
        Feb 19, 2020 10:00 AM
        Feb 26, 2020 10:00 AM
        Mar 4, 2020 10:00 AM
        Mar 11, 2020 10:00 AM
        Mar 18, 2020 10:00 AM
        Mar 25, 2020 10:00 AM
        Apr 1, 2020 10:00 AM
        Apr 8, 2020 10:00 AM
        Apr 15, 2020 10:00 AM
        Apr 22, 2020 10:00 AM
        Apr 29, 2020 10:00 AM
        May 6, 2020 10:00 AM
        May 13, 2020 10:00 AM
        May 20, 2020 10:00 AM
        May 27, 2020 10:00 AM
        Jun 3, 2020 10:00 AM
Please download and import the following iCalendar (.ics) files to your calendar system.
Weekly: https://zoom.us/meeting/upMlf-irpj0jBlE_mNDX7bi95FplcdsYlA/ics?icsToken=98tyKu6vpjMrGdyTuVzHf6ovW4X_bvGxiFNCj5xQsjvdVCJaO1bQJu5BNqFGHPmB

Join Zoom Meeting
https://zoom.us/j/778813869

Meeting ID: 778 813 869

One tap mobile
+16465588656,,778813869# US (New York)
+17207072699,,778813869# US (Denver)

Dial by your location
        +1 646 558 8656 US (New York)
        +1 720 707 2699 US (Denver)
Meeting ID: 778 813 869
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
 - Information about Clients (apps) to which users have granted access and what specific access they have delegated should not be made available to Resource Servers that do not enforce relevant access restrictions on those Clients.
