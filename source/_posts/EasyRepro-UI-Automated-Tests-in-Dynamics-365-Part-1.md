---
title: EasyRepro - UI Automated Tests in Dynamics 365 - Part 1
date: 2018-03-05
tags: 
- Dynamics
- Testing
---
Many, perhaps most, software applications today are written as web-based applications to be run in an Internet browser. The effectiveness of testing these applications varies widely among companies and organizations. In an era of highly interactive and responsive software processes where many organizations are using some form of Agile methodology, test automation is frequently becoming a requirement for software projects. Test automation is often the answer. Test automation means using a software tool to run repeatable tests against the application to be tested. For regression testing this provides that responsiveness.

There are many advantages to test automation. Most are related to the repeatability of the tests and the speed at which the tests can be executed. There are a number of commercial and open source tools available for assisting with the development of test automation. [Selenium](https://www.seleniumhq.org/) is possibly the most widely-used open source solution. 

<!-- more -->

There are other alternatives, some of them are tools built on top Selenium, others are simply frameworks that repeat the same concept; a programmable API that lets you control de Browser by faking the operations that a user would perform while navigating through the application pages. Some of these are:

- https://www.ranorex.com
- https://www.telerik.com/teststudio
- https://leaptest.com
- https://www.codeproject.com/Tips/658947/Watin-An-Automation-Testing-in-NET


# When to Automate UI Tests
It is not always advantageous to automate test cases. There are times when manual testing may be more appropriate. For instance, if the application’s user interface will change considerably in the near future, then any automation might need to be rewritten anyway. Also, sometimes there simply is not enough time to build test automation. For the short term, manual testing may be more effective. If an application has a very tight deadline, there is currently no test automation available, and it’s imperative that the testing get done within that time frame, then manual testing is the best solution.


# Selenium
[Selenium](https://www.seleniumhq.org/) is a set of tools and libraries meant to automate and run different browsers. It has different components, one of which is the WebDriver. 

The [WebDriver](https://www.seleniumhq.org/projects/webdriver/) is a library, very well designed and easy to understand that provides access to the different components and actions that most browsers have available. 

Probably the biggest challenge in designing such a tool is the overwhelming variety of Browsers and Platforms available in the market. So, how can we find the common ground among them so we can write test specifications once and run them in different browsers? But not only that, also several combinations of browsers and Operating Systems? 

{% asset_img WebDriver_3.png WebDriver for different browsers %}

Even more, how can we make these libraries available to different coding languages, so we can write the tests in the same way no matter if we are using c#, Java or Python?

{% asset_img WebDriver_2.png WebDriver for different browsers %}

Selenium and the WebDriver tackle above problems providing an elegant API built in several layers. 

# Example

Let's analyze this simple piece of code to see the different elements we are talking about

```c#
[Fact]
public void GoogleSearch()
{
    // 1. Initialize the Driver
    using (var driver = new ChromeDriver())
    {
        // 2. Go to the "Google" homepage
        driver.Navigate().GoToUrl("http://www.google.com");

        // 3. Find the search box on the page
        var searchBox = driver.FindElementById("lst-ib");

        // 4. Enter the text to search for
        searchBox.SendKeys("Selenium Test");

        // 5. Find the search button
        var searchButton = driver.FindElementByName("btnK");

        // 6. Click on it to start the search
        searchButton.Submit();

        // 7. Find the "Id" of the "Div" containing results stats,
        // located just above the results table.
        var searchResults = driver.FindElementById("resultStats");

        // 8. TODO: Perform any operations to make sure the behavior is correct 
        Assert.Contains("seconds", searchResults.Text);
    }
}
```

First of all, Selenium is available to be coded using different languages. Each language/platform require you to download and reference the appropriate libraries. In this case, we are using Selenium for c# which is available as a nuget package for Visual Studio. You can download it from here: https://www.nuget.org/packages/Selenium.WebDriver/

If I run the test above I get the following result:

{% asset_img SeleniumBasicTest.gif Selenium execution result %}


Let's move onto analyzing the code. In summary, what this code does is opening a Chrome browser window, navigate to the Google page and perform a search. Easy, right?

```c#
// 1. Initialize the Driver
using(var driver = new ChromeDriver())
```
This is how you tell Visual Studio you want to automate the **Chrome** Browser. If we wanted to automate other browsers, there are specific Objects for them.

The line above initializes and gets the object that will let you interact with Chrome. 

But wait, didn't we say before that we are able to write a test and run it in different browsers and operating systems? 
Why do I need to reference Chrome specifically? 
What if I want to run the same test in Internet Explorer? Do I need to write the same test again?

There is in fact a way to initialize a generic browser and only specify which particular one to use at runtime by a configuration file or any other mechanism. 
For the sake of simplicity, I'm creating a specific reference to Chrome, but the same concepts apply to the other available drivers. 

```c#
// 2. Go to the "Google" homepage
driver.Navigate().GoToUrl("http://www.google.com");
```
This is how we tell the browser to go an type a web address in the navigation bar and hit enter. 

```c#
// 3. Find the search box on the page
var searchBox = driver.FindElementById("lst-ib");
```
Here we are saying, go find me the search text box. 
If we were testing any other type of application, for instance a record form, this would be the way to go find a field in that form and type a value on it later.
If that text box that is not available, because is hidden, or it wasn't rendered, an error will be thrown and the test will fail. 

```c#
// 4. Enter the text to search for
searchBox.SendKeys("Selenium Test");
```
Once we got the text box, we can type any value we want on it.

```c#
// 5. Find the search button
var searchButton = driver.FindElementByName("btnK");
```
The same way we had to find the search box, now we have to find the search button. This is a very common pattern in Selenium. You first search for the UI element you want to interact with, and then perform the interaction, for instance typing something, clicking on it, swipe, select a value from a drop-down list, or do whatever you need to. Every type of UI element offer different type of interactions.

```c#
// 6. Click on it to start the search
searchButton.Submit();
```
And Click on it. At this stage, the browser will connect to the server, send a request and render the results. Even though this sometimes happens very fast, especially in the Google search, it takes certain amount of time. 
If I perform the next activity on the test immediately after clicking the button, the UI elements may not be present just yet, and my test would fail. 
So, how do I make sure the page code has finished rendering or performing the operations before I continue running the tests?

Selenium provides a waiting mechanism that is transparent, but configurable. Every time it detects the browser is performing a task, like a server request, it waits until it is completed before moving to the next step. 
Sometimes, this is not enough, and we need to wait longer. 
For those cases, there are other available API functions exposed to perform waits.

```c#
// 7. Find the "Id" of the "Div" containing results stats,
// located just above the results table.
var searchResults = driver.FindElementById("resultStats");
```
Now that the search is completed, I can add code to check if the results are the ones that I expected. 
At this point, I can use the normal Assert methods available in any test framework to make the test fail if there's something wrong in the result. 
The way I make sure the page has the right values or the correct state is by the mechanism explained in #4: Search the page UI element, then get the contained value. 

```c#
// 8. TODO: Perform any operations to make sure the behavior is correct 
Assert.Contains("seconds", searchResults.Text);
```
Once I have the reference to the DIV element, I can read and check its contents. 

The above code is available on Github. Feel free to send a Pull Request if you see any errors or want to suggest any improvements. You can also clone the repository and run the tests locally if you want to try it yourself.
https://github.com/crisfervil/EasyReproTest/blob/master/src/EasyReproTest/SeleniumBasicExample.cs



# EasyRepro

In this first part we covered an introduction of the main elements of the Browser User Interface Test. The next chapter (#Add next chapter link here#) we are going to describe a library built on top of Selenium specifically designed to help us write UI tests for Dynamics CRM. 

This library built by Microsoft takes advantage of the well defined structure of Entities, Attributes, SiteMap and all the different elements that are part of the platform.

Continue reading the second part here: {% post_link EasyRepro-UI-Automated-Tests-in-Dynamics-365-Part-2 Part 2 %}


# References
- https://msdn.microsoft.com/en-us/library/dd286726.aspx
- https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing
