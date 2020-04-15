---
title: MB-200 notes
tags: 
- Dynamics
- Certifications
---
Notes I've taken during the preparation of the [MB-200: Microsoft Power Platform + Dynamics 365 Core](https://docs.microsoft.com/en-us/learn/certifications/exams/mb-200?tab=tab-learning-paths) exam


# Apps

## To setup access to apps
* Go to Settings > My Apps.
* In the lower-right corner of the app tile you want to manage access for, select More options (...), and then select Manage Roles.
* Choose whether you want to give app access to all security roles or selected roles

https://docs.microsoft.com/en-us/dynamics365/customerengagement/on-premises/customize/manage-access-apps-security-roles

# Change Tracking

The change tracking feature in Common Data Service provides a way to keep the data synchronized in an efficient manner by detecting what data has changed since the data was initially extracted or last synchronized. 

This option must be set at entity level

https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/use-change-tracking-synchronize-data-external-systems

# Flow

In order to trigger a flow when a record changes, you need to enable the change tracking for the entity.


# Data Performance 
[Data Performance](https://www.powerobjects.com/blog/2017/10/30/managing-your-data-performance-in-dynamics-365/) in Dynamics 365
Available in Settings>Administration>Data Performance
The **Optimization Impact** will show how the optimization is impacting the performance. *Negative* values improve the performance and *positives* degrades it. 

This is a featue that only existed in V8.
[It was removed on V9](https://trainingsupport.microsoft.com/en-us/mcp/forum/all/mb-200-exam-item-challenge-questions-regarding/a5c924bb-3268-40c6-ae9a-4ffd8fd92614)

It was replaced by a feature called [Automatic Tuning](https://docs.microsoft.com/en-us/power-platform/admin/analyze-improve-data-query-performance)

# One Note

Steps to integrate with One Note:
- Step 1: Turn on server-based SharePoint integration
- Step 2: Turn on OneNote integration

https://docs.microsoft.com/en-us/power-platform/admin/set-up-onenote-integration-in-dynamics-365

# Dynamics 365 App for Outlook

## To push the app to users
* Go to Settings > Dynamics 365 App for Outlook.
* Do one of the following:
    - To push the app to all eligible users, click Add App for All Eligible Users.
    - To push the app to certain users, select those users in the list, and then click Add App to Outlook.

Make sure to add users to the security role Dynamics 365 App for Outlook User as described in the Provide security role access section above.

## To have users install the app themselves
* Users click the Settings button Settings button, and then click Apps for Dynamics 365 apps.
* In the Apps for Dynamics 365 apps screen, under Dynamics 365 App for Outlook, users click Add app to Outlook.

https://docs.microsoft.com/en-us/dynamics365/outlook-app/deploy-dynamics-365-app-for-outlook

# Office 365 Groups

Users must have an Exchange Online mailbox set up to use Office 365 Groups. Exchange Online is already properly configured for organizations as a part of Office 365. You also need to enable server-based SharePoint integration to see documents in an Office 365 Group; you don't have to use SharePoint integration, only set up the connection to SharePoint Online. Server-based SharePoint integration is also required to enable the group OneNote notebook.

## Steps to integrate

* Create or choose an Office 365 Group.
* Select Connectors. Scroll down to Dynamics 365 Online, and then select Add.
* If you have access to more than one environment, choose which environment to connect to this Office 365 Group. If you only have access to one environment, this step will be skipped and you will advance to the next step.
* Choose the record you want to connect this Office 365 Group to, and then select Save.

https://docs.microsoft.com/en-us/power-platform/admin/deploy-office-365-groups

# Voice of the Customer

## How to install
- Go to the Dynamics 365 Administration Center, and then select the Applications tab.
- Select the application row titled Voice Of The Customer, and then select Manage.
- From the Dynamics 365 Instance drop-down list, select the instance where you want to install the solution.

https://docs.microsoft.com/en-us/dynamics365/voice-of-customer/install-solution
https://docs.microsoft.com/en-us/learn/paths/dyn365-voice-of-customer/

# Metadata Diagram Tool

There is a tool that will programmatically generate Office Visio metadata diagrams

Usage:

``` console
MetadataDiagramConsole.exe new_bankaccount new_safedepositbox  
```

https://docs.microsoft.com/en-us/dynamics365/customerengagement/on-premises/developer/use-metadata-generate-entity-diagrams