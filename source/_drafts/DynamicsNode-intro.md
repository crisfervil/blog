---
title: DynamicsNode intro
date: "2017/04/04 23:50"
tags:
---
# Dynamics CRM Coding made simple

How many steps do you need to create a console application to, for example, create a record in Dynamics CRM?

- Open Visual Studio
- Create a new Console Application Project
- Add Microsoft.CrmSdk.CoreAssemblies and Microsoft.CrmSdk.XrmTooling.CoreAssembly Nuget Packages
- Start Coding (code below)
- F5 to run your code
 
``` c#
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Tooling.Connector;

namespace CRMTest
{
    class Program
    {
        static void Main(string[] args)
        {
            CrmServiceClient conn = new CrmServiceClient("Url=https://contoso.crm.dynamics.com; Username=someone@contoso.onmicrosoft.com; Password=password; authtype=Office365");
            IOrganizationService orgService = conn.OrganizationServiceProxy;

            var account = new Entity("account");
            account["name"] = "Fourth Coffee";
            account["AccountCategoryCode"] = new OptionSetValue((int)12345);
            account["CustomerTypeCode"] = new OptionSetValue((int)12344);

            // Create an account record named Fourth Coffee.
            var accountId = orgService.Create(account);

        }
    }
}

```

Not only this. Every time you create and compile a project with visual studio, a long list of files are generated. 

For the previous example, getting a list of files in the directory returns 104 items. 

Don't get me wrong, Visual Studio and the .net Framework are great, but for small tasks like this it just requires too many steps. 

And that's one of the reasons I've created DynamicsNode; to be able to easily create scripts, run them, share them if needed, and move to the next task. 


Let's see now how many stepts it requires to do the same wihing With DynamicsNode:

- Create text file myscript.js
- Add code to the created file using your preferred code editor  (code below)
- Run your code, by typing ``` node myscript.js``` in the command prompt 

``` js
var dn = require('dynamicsnode');

var crm = new dn.CRMClient('Url=https://contoso.crm.dynamics.com; Username=someone@contoso.onmicrosoft.com; Password=password; authtype=Office365');

var account = {Name:'Fourth Coffee', AccountCategoryCode:'Preferred Customer', CustomerTypeCode:'Investor'};
var accountId = crm.create('account',account);

```
And that's it. The best part is that you have had to create only one file, and that's all you need. If you need to deploy this somewhere else, or share the code with someone else, it's super easy.

Apart from the easyness, DynamicsNode has a lot more of advantages, like for example and JSON-like query language, that allows you to query CRM like this:
```js
// This returns all the Accounts which type is Investor. You don't need to remember the Option
var accounts = crm.retrieveMultiple('account',{CustomerTypeCode:'Investor'});


``` 

References:
- https://msdn.microsoft.com/en-us/library/jj602970.aspx
