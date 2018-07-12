---
title: UI-Automated-Tests-in-Dynamics-CRM-365-Part3
tags:
---
# Intro
This is the third part of a 3 parts intro article about Automated User Interface tests for Dynamics 365. 

The first part [Link to the First part] describes what is the purpose of automating UI tests and when is convenient to do it. Also, we talk about [Selenium](https://www.seleniumhq.org/), probably the most popular and robust tool available for Browser UI automation. 

In the second part [Link to the Second part] we introduce [EasyRepro](https://github.com/Microsoft/EasyRepro), a framework built on top of Selenium that help us to write UI Tests for Dynamics 365. 

In this part we are describing EasyRepro, a library built by Microsoft that leverages all the power provided by Selenium and takes advantage of the well defined structure of components in Dynamics 365.

In this part, we are going to see another example, and discuss some desirable features in any User Interface Automated test. 

# Global Search Test Example

https://github.com/Microsoft/EasyRepro/blob/releases/v9.0/Microsoft.Dynamics365.UIAutomation.Sample/GlobalSearch.cs

``` c#
    [Fact]
    public void GlobalSearchTest()
    {
        // 1 - Create instance of the browser
        using (var xrmBrowser = new Browser(BrowserType.Chrome))
        {
            var url = new Uri("http://orgname.crm.dynamics.com");
            var userName = "admin@youruser.onmicrosoft.com".ToSecureString();
            var pwd = "yourpassword".ToSecureString();

            // 2. Log-in to Dynamics 365
            xrmBrowser.LoginPage.Login(url, userName, pwd);

            // 3 - Close Guided Help
            xrmBrowser.GuidedHelp.CloseGuidedHelp();

            // 4 - Wait for it
            xrmBrowser.ThinkTime(500);

            // 5 - Perform the search from the navigation bar
            xrmBrowser.Navigation.GlobalSearch("contoso");

            // 6 - Perform the search from the search page
            xrmBrowser.GlobalSearch.Search("Contoso");

            xrmBrowser.ThinkTime(4000);

            // Other functionality available in Global Search
            xrmBrowser.GlobalSearch.FilterWith("account");
            xrmBrowser.GlobalSearch.OpenRecord("account",1);
        }
    }
```


# Desirable features in UI Tests

Telemetry, Logging and screenshots. 

Configurable. Write once, run in multiple platforms.

# How to run EasyRepro from VS Online

How to integrate it in the CI/CD pipeline

https://www.visualstudio.com/team-services/continuous-integration/

# References


# Test Asynchronous Plugins/Workflows

Proposal of how to check the result of async plugins triggered during the UI test

For async plugins, probably the best option is to unit test it using FakeXrmEasy or similar.

For async Workflows, an option can be to create an integration test, and kick off the WF independently, and wait for it to complete. 
https://msdn.microsoft.com/en-us/library/microsoft.crm.sdk.messages.executeworkflowrequest.aspx
https://msdn.microsoft.com/en-us/library/gg309600.aspx
