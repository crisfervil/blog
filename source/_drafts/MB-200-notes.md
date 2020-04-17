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


## Solutions

There are managed and unmanaged solutions. A managed solution cannot be modified and can be uninstalled after it is imported. All the components of that solution are deleted by uninstalling the solution.

When you import an unmanaged solution, you add all the components of that solution into your environment. You can’t delete the components by uninstalling the solution.

When you import an unmanaged solution that contains components that you have already customized, your customizations will be overwritten by the customizations in the imported unmanaged solution. You can’t undo this.

https://docs.microsoft.com/en-us/powerapps/maker/common-data-service/solutions-overview


## Search

There are three types of search:
- Relevance Search
- Full-text Quick Find (single-entity or multi-entity)
- Quick Find (single-entity or multi-entity)

Relevance Search uses the same default scoring concepts as Azure Search. Scoring refers to the computation of a search score for every item returned in search results

Data in the application begins syncing to the external search index immediately after you enable Relevance Search. Is recommended to configure the entities and entity fields participating in Relevance Search before enabling the search, to prevent sensitive data from being indexed in a service external to model-driven apps in Dynamics 365.

https://docs.microsoft.com/en-us/power-platform/admin/configure-relevance-search-organization

Categorized Search is the old version of the Relevance Search, and is available on-prem. Relevance search is available online only. 

Categorized Search uses SQL text catalogs to work. 

https://crmbook.powerobjects.com/basics/searching-and-navigation/searching-crm/#categorized_search

# Change Tracking

The change tracking feature in Common Data Service provides a way to keep the data synchronized in an efficient manner by detecting what data has changed since the data was initially extracted or last synchronized. 

This option must be set at entity level

https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/use-change-tracking-synchronize-data-external-systems

# Flow

In order to trigger a flow when a record changes, you need to enable the change tracking for the entity.


# Administration

## Users
In order to change a user's logon name, you have to do it thorough the Office 365 admin center
https://docs.microsoft.com/en-us/microsoft-365/admin/add-users/change-a-user-name-and-email-address?view=o365-worldwide

## Sessions

To configure the session timeout:
- In the Power Platform admin center, select an environment.
- Select Settings > Product > Privacy + Security.
- Set Session Expiration and Inactivity timeout. 

https://docs.microsoft.com/en-us/power-platform/admin/user-session-management


In older versions, use System Settings > General Tab > Set Session Timeout

# Excel Export
The export to an Excel dynamics worksheet has a limit of 100.000 records, and is not available to all record types
https://docs.microsoft.com/en-us/dynamics365/customerengagement/on-premises/basics/export-excel-dynamic-worksheet

When you export to a dynamic worksheet or PivotTable, a link is maintained between the Excel worksheet and Dynamics 365 (online). Every time a dynamic worksheet or PivotTable is refreshed, you’ll be authenticated with Dynamics 365 (online) using your credentials. You’ll be able to see the data that you have permissions to view.

# Marketing Lists
Two types of Marketing lists: Dynamic and Static.

You can add up to 120,000 members to a static marketing list in one Add operation. If you need to add more than 120,000 members to a static marketing list, split those members into multiple add operations.

https://docs.microsoft.com/en-us/dynamics365/sales-enterprise/create-marketing-list-using-app-marketing-sales


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

[Deploy Office 365 Groups Dynamics 365 (online)](https://docs.microsoft.com/en-us/power-platform/admin/deploy-office-365-groups)

[Collaborate with your colleagues using Office 365 Groups](https://docs.microsoft.com/en-us/dynamics365/customerengagement/on-premises/basics/collaborate-with-colleagues-using-office-365-groups)

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


# Data Performance 
[Data Performance](https://www.powerobjects.com/blog/2017/10/30/managing-your-data-performance-in-dynamics-365/) in Dynamics 365
Available in Settings>Administration>Data Performance
The **Optimization Impact** will show how the optimization is impacting the performance. *Negative* values improve the performance and *positives* degrades it. 


# Dynamics 365 Diagnostic Tool
You can access it throug a URL (tools/diagnostics/diag.aspx)
It shows latency information, bandwidth, as well as benchmark test results. 
https://community.dynamics.com/365/b/adrianbegovichblog/posts/dynamics-365-diagnostics-tool
https://docs.microsoft.com/en-us/power-platform/admin/verify-network-capacity-throughput-clients


# Organization Insights Tool

(Old way)
To enable it, do it through the Dynamics Marketplace. Then access it from the organization settings
https://www.powerobjects.com/blog/2018/03/12/how-install-view-organization-insights-dynamics-365/
This has been deperecated. 

The new Organization Insights is available in Power Platform Admin Center, under the Analytics section. 
It shows the active users, API calls, number of pluging executions, plugin errors, etc.
https://docs.microsoft.com/en-us/power-platform/admin/analytics-common-data-service
