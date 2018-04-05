---
title: UI-Automated-Tests-in-Dynamics-CRM-365-Part2
tags:
---
# Intro
This is the second part of a 2 parts intro article about Automated User Interface tests for Dynamics 365. 

The first part [Link to the First part] describes what is the purpose of automating UI tests and when is convenient to do it. Also, we talk about [Selenium](https://www.seleniumhq.org/), probably the most popular and robust tool available for Browser UI automation. 

In this part we are describing EasyRepro, a library built by Microsoft that leverages all the power provided by Selenium and takes advantage of the well defined structure of components in Dynamics 365.

# EasyRepro

As you've seen, Selenium is a comprehensive robust tool that will allow you to write User Interface tests for Dynamics CRM. It has all you need, but it becomes quite laborious to write medium and complex tests. 

Every click and form field filling requires a few lines of code. [There are patterns](https://saucelabs.com/blog/selenium-design-patterns) to help you increase the reusability of your tests to the maximum, but still, some tests can be hard and time consuming to write. 

But at the same time Dynamics 365 has a feature that comes in really handy to reduce the amount of code required; every single form, section and field is rendered in the same way, it follows the same structure, and it has a perfectly defined naming convention for UI elements. 

This increases the chances of writing reusable code. If you think about it, given an entity and the list of attributes in it, you know what the URL of a form will be, and how the UI elements are going to be named. If you also know the attribute data types, you know what type of elements (text box, option set, dropdown) are going to be rendered for each attribute.

Well, the clever guys at Microsoft must have came up with the same conclusion, and decided to create a framework for this; it is called [EasyRepro](https://github.com/Microsoft/EasyRepro).

EasyRepro is a library meant to facilitate the writing of UI tests for Dynamics 365, leveraging the well defined structure mentioned above. 

# Example

Let's see a small example and see how this would work.

First of all, the library is distributed through a nuget package that can be downloaded from here: https://www.nuget.org/packages/Dynamics365.UIAutomation.Api/


```c#
[Fact]
public void CreateContact()
{
    // 1. Create instance of the browser
    using (var xrmBrowser = new XrmBrowser(new BrowserOptions() { BrowserType=BrowserType.Chrome }))
    {
        var url = new Uri("http://orgname.crm.dynamics.com");
        var userName = "admin@youruser.onmicrosoft.com".ToSecureString();
        var pwd = "yourpassword".ToSecureString();
        // 2. Log-in to Dynamics 365
        xrmBrowser.LoginPage.Login(url, userName, pwd);

        xrmBrowser.ThinkTime(500);

        // 3. Go to Sales/Accounts using the Sitemap
        xrmBrowser.Navigation.OpenSubArea("Sales", "Contacts");

        xrmBrowser.ThinkTime(500);

        // 4. Change the active view
        xrmBrowser.Grid.SwitchView("Active Contacts");

        xrmBrowser.ThinkTime(500);

        //5. Click on the "New" button
        xrmBrowser.CommandBar.ClickCommand("New");

        xrmBrowser.ThinkTime(1000);

        var fields = new List<Field>
        {
            new Field() {Id = "firstname", Value = "Test"},
            new Field() {Id = "lastname", Value = "Contact"}
        };

        //6. Set the attribute values in the form
        xrmBrowser.Entity.SetValue(new CompositeControl() { Id = "fullname", Fields = fields });
        xrmBrowser.Entity.SetValue("emailaddress1", "test@contoso.com");
        xrmBrowser.Entity.SetValue("mobilephone", "555-555-5555");
        xrmBrowser.Entity.SetValue("birthdate", DateTime.Parse("11/1/1980"));
        xrmBrowser.Entity.SetValue(new OptionSet { Name = "preferredcontactmethodcode", Value = "Email" });

        //7. Save the new record
        xrmBrowser.CommandBar.ClickCommand("Save");
    }
}
```

If we run the test above, this would be the result:

![EasyRepro Test](images/EasyReproTest.gif)

Now let's analyze the most important parts:

```c#
// 1. Create instance of the browser
using (var xrmBrowser = new XrmBrowser(new BrowserOptions() { BrowserType=BrowserType.Chrome }))
```
This creates an instance of the XrmBrowser, which is the main object that will contain the methods to interact with Dynamics. This is also the main difference with normal Selenium tests; I no longer need to create a specific WebDriver object for each browser type. EasyRepro sets this XrmBrowser object which will do all the job for us. I only need to specify what Browser I want to run against as a parameter in the constructor. 

This value along with the url and user credentials, can and must ne read from a configuration source, like the app.config file.

```c#
// 2. Log-in to Dynamics 365
xrmBrowser.LoginPage.Login(url, userName, pwd);
```
This is just how we tell to the Browser we want to perform the necessary steps to log in into the application. 

Again, this is the cool part of EasyRepro. Given that the Login page, its URL and all the UI elements are the same for any given CRM Online instance, I can encapsulate all of the logic for performing that interaction in a function, and just use it providing the relevant information. In this case, the url, user and password. 

```c#
// 3. Go to Sales/Accounts using the Sitemap
xrmBrowser.Navigation.OpenSubArea("Sales", "Contacts");
```

It's the same story here. The navigation and the sitemap are made of several UI elements, and in order to click on any of them I need to know how's that structure rendered and how are its parts glue together. With EasyRepro, I only need to provide what's relevant. That I want to click on the **Sales** area of the site map and the **Contacts** section of it. EasyRepro will take care of the rest. 

```c#
// 4. Change the active view
xrmBrowser.Grid.SwitchView("Active Contacts");
```
After clicking on the desired sitemap item, the browser will navigate to the main view of the contacts entity. The main view can be the default one, which may or may not ne the one that I want to use, so with the line above, I make sure that view is selected. 

```c#
//5. Click on the "New" button
xrmBrowser.CommandBar.ClickCommand("New");
```
The CommandBar object provides the ClickCommand method to interact with the Ribbons. In this case, we provide the Text on the button we want to click in. It also works with dropdown buttons.

```c#
//6. Set the attribute values in the form
xrmBrowser.Entity.SetValue(new CompositeControl() { Id = "fullname", Fields = fields });
xrmBrowser.Entity.SetValue("emailaddress1", "test@contoso.com");
```
Now comes the interesting stuff. Where we set the values of the record. The account entity is a good example of interaction with composite fields, where the name in this case is made of other parts. 

```c#
//7. Save the new record
xrmBrowser.CommandBar.ClickCommand("Save");
```
And finally, as we've seen before, we can use the **CommandBar** object to click on the **Save** button.

And now what?

The next step in this test should be to check the status of the records. The usual requirement when creating a record is to set some values by default, so we would like to make sure these fields were populated correctly. 

The way to do it is through the normal Assert methods available in any test framework. 

In order to read the value of an attribute you would need something like
```c#
var url = xrmBrowser.Entity.GetValue("websiteurl");
``` 

The above code is available on Github. Feel free to send a Pull Request if you see any errors or want to suggest any improvements. You can also clone the repository and run the tests locally if you want to try it yourself.
https://github.com/crisfervil/EasyReproTest/blob/master/src/EasyReproTest/ContactTests.cs

There are also more examples available in the EasyRepro github. 
https://github.com/Microsoft/EasyRepro/tree/releases/v9.0/Microsoft.Dynamics365.UIAutomation.Sample

# References
https://msdn.microsoft.com/en-us/library/dd286726.aspx
https://www.seleniumhq.org/docs/01_introducing_selenium.jsp
https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing
http://toolsqa.com/selenium-c-sharp/
https://www.codeproject.com/Articles/1016775/Getting-Started-with-WebDriver-Csharp-in-Minutes
https://crmtipoftheday.com/967/ui-testing-for-dynamics-365/