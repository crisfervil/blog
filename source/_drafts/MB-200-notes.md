---
title: MB-200 notes
tags: 
- Dynamics
- Certifications
---
Notes I've taken during the preparation of the [MB-200: Microsoft Power Platform + Dynamics 365 Core](https://docs.microsoft.com/en-us/learn/certifications/exams/mb-200?tab=tab-learning-paths) exam


# Training Material

|Duration|Topic
|---|---
|02:11|[Canvas Apps](https://docs.microsoft.com/en-ie/learn/paths/create-powerapps/)
|02:40|[Model driven apps](https://docs.microsoft.com/en-ie/learn/paths/create-app-models-business-processes/)
|05:15|[Implementing Customer Engagement](https://docs.microsoft.com/en-ie/learn/paths/implementing-customer-engagement-apps/)
|03:20|[Power Automate](https://docs.microsoft.com/en-ie/learn/paths/automate-process-power-automate/)
|06:22|[Power BI](https://docs.microsoft.com/en-ie/learn/paths/create-use-analytics-reports-power-bi/)
|19:48|Total

https://docs.microsoft.com/en-us/learn/certifications/exams/mb-200

# Power Apps

Power Apps Studio: To configure user interface (UI) elements.
Power Apps Admin Center: To define environment and data policies.
make.powerapps.com: To open existing apps, create, share apps, and create data connections and flows.

You can use many external data sources, including Twitter, Facebook, Microsoft SQL Server, and Salesforce. You can use also on-prem data using the data gateway. 

When creating a new App from a data source, the Browse, Edit and Details screens are automatically created. 

When a user selects a button or a control, the OnSelect event is triggered. Then you have to set the formula that you want to run. 

You can customize all aspects of the app, including screen, layouts and formulas, when creating an app from data. 

The gallery control allows to display and select all records from a data source.

Formula reference for Power Apps: https://docs.microsoft.com/en-ie/powerapps/maker/canvas-apps/formula-reference

People who have Co-owner permission also need a Power Apps P2 license to work directly with entities in Common Data Service.

If you want to work with Power Apps environments, you need a Power Apps Plan 2 license or the free Power Apps Plan 2 trial. Additionally, if you want to work with Dynamics 365 restricted entities, you must have a Power Apps for Dynamics 365 license. [Licensing overview for Power Platform](https://docs.microsoft.com/en-us/power-platform/admin/pricing-billing-skus)

Customer Survey is not created in your environment when you toggle "Include sample apps and data" to true.

You can't create a custom activity by using the Microsoft Power Apps portal. You must open Solution Explorer by using the Advanced button.

Things that you can do with Business Rules:
- Set field values.
- Clear field values.
- Set field requirement levels.
- Show or hide fields.
- Enable or disable fields.
- Validate data and show error messages.
- Create business recommendations based on business intelligence.

The below is not available on canvas apps:
- Show or hide fields
- Enable or disable fields
- Create business recommendations based on business intelligence.

You cannot set a lookup value when importing data from Excel, this must be done from Power Apps.

A tenant can have up to 75 non-Production instances and 50 Production instances. 

You cannot restore backups into a Production instance, the instance will have to be switched to Sandbox first, then you can proceed restoring from the backup, and then switch to Production again.

# Model Driven Apps

To share an app, you must have the Environment Admin or System Admin role.

System Customizers have full permission to customize the environment. But users who have this role can view records only for environment entities that they create. To learn more, see [Privileges required for customization](https://docs.microsoft.com/dynamics365/customer-engagement/customize/privileges-required-customization).

You can have up to 10 active business process flows per entity.


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



# Solution Patching and Versioning

* **Solution Segmentation** allows to export solutions with selected entity assets, such as entity fields, forms, and views, rather than entire entities with all the assets.

* A solution’s version has the following format: major.minor.build.revision. 

* A patch must have a higher build or revision number than the parent solution. It can’t have a higher major or minor version. The display name can be different.

* A cloned solution must have the version number greater than or equal to the version number of the base solution. 

* **Clone to patch** creates a patch of the main solution.

* **Clone to solution** creates a new version, that includes all the patches from the main solution. A cloned solution must have the version number greater than or equal to the version number of the base solution

* A patch represents an incremental minor update to the parent solution. A patch can add or update components and assets in the parent solution when installed on the target system, but it can’t delete any components or assets from the parent solution.

* A patch can have only one parent solution, but a parent solution can have one or more patches.

* A patch is created for unmanaged solution. You can’t create a patch for a managed solution.

* When you export a patch to a target system, you should export it as a managed patch. Don’t use unmanaged patches in production environments.

* The parent solution must be present in the target system to install a patch.

* You can delete or update a patch.

* If you delete a parent solution, all child patches are also deleted. The system gives you a warning message that you can’t undo the delete operation. The deletion is performed in a single transaction. If one of the patches or the parent solution fails to delete, the entire transaction is rolled back.

* After you have created the first patch for a parent solution, the solution becomes locked, and you can’t make any changes in this solution or export it. However, if you delete all of its child patches, the parent solution becomes unlocked.

* When you clone a base solution, all child patches are rolled up into the base solution and it becomes a new version. You can add, edit, or delete components and assets in the cloned solution.

* A cloned solution represents a replacement of the base solution when it’s installed on the target system as a managed solution. Typically, you use a cloned solution to ship a major update to the preceding solution.

https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/create-patches-simplify-solution-updates

https://community.dynamics.com/365/f/dynamics-365-general-forum/348379/solution-version-number/932999

https://d365demystified.com/2019/01/05/using-clone-a-patch-clone-solution-in-d365-solutions/

https://docs.microsoft.com/en-us/dynamics365/customerengagement/on-premises/customize/use-segmented-solutions-patches-simplify-updates


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

# OneDrive

## Requirements to integrate:

- A OneDrive for Business license for each use
- A SharePoint license for each user.
- Office 365 Enterprise E3 or later

## Enabling OneDrive

- Settings > Integration > Document management settings
- Enable OneDrive for Business to enable it, and then select OK
- Add permissions to the user. Edit a Security Role. Under Miscellaneous Privileges, toggle the OneDrive for Business privilege to the desired availability

https://docs.microsoft.com/en-us/power-platform/admin/enable-onedrive-for-business

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
This has been deprecated. 

The new Organization Insights is available in Power Platform Admin Center, under the Analytics section. 
It shows the active users, API calls, number of plugin executions, plugin errors, etc.
https://docs.microsoft.com/en-us/power-platform/admin/analytics-common-data-service
