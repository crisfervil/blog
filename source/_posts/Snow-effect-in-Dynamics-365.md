---
title: Snow effect in Dynamics 365
date: 2018-12-06
tags: 
- Dynamics
- JS
---
{% asset_img banner.png Links Banner %}
---

So, Christmas is around the corner, and even though there are some grinches out there is good to keep the spirit alive and spread the good vibes. 

And because we are D365 hackers and we can't help being obsessed with our work, we have to focus that energy to produce something different and really *useful*. 

Today, a small hack to bring the Christmas winter atmosphere to our deployments: *The snow effect*.

Based on Scott Schiller snow storm project:  https://github.com/scottschiller/snowstorm/

<!-- more -->


This trick works well on on-line instances. I haven't tested it on an on-premise, but there is no reason why it shouldn't work. 

I haven't tested it on Internet Explorer or Firefox either. It works well on Chrome and Edge.

**Disclaimer**: Of course this is an **un**supported customization and it must not be done in production. Do it on your DEV environments with the sole purpose of amusing your developer fellows. 

{% asset_img Capture01.gif Snow Effect %}

## Steps

1 - Create a web resource called new_snow.js in your instance

Copy and paste the code available on https://raw.githubusercontent.com/scottschiller/Snowstorm/master/snowstorm.js

You will see that the script contains some snow properties on the top of the file that can be configured.

Adjust the **snowColor** to something like ```'lightblue'``` or ```'#9dd0e1'```. The vanilla CRM background is completely white, so white on white doesn't work. 

Also, you have to change the **freezeOnBlur** property, because all the CRM UI is based on IFRAMES, and this property stops the snow effect when the container IFRAME doesn't have the focus. 

{% asset_img Capture04.png Snow Effect configuration %}


2 - Find a suitable web resource to call the above script from by hitting F12 and checking the web resources loaded on the top IFRAME.

This is the hacking part, and of course you can apply the same idea to get other results. 

{% asset_img Capture02.png Developer Tools Web Resources %}


In this example, the victim can be the *LoadGuidedHelp.js* highlighted in the screenshot. 

3 - Open the Default Solution, and find the web resource above by filtering the webresources view

{% asset_img Capture03.png Web Resources view %}


4 - Add the following piece of code to the file, to call our snow effect file

```js
// snow script. remove after Christmas
var script = document.createElement('script');
script.src = "/webresources/new_snow.js";
document.head.appendChild(script);
// end snow script
```
The modified file will look something like...

{% asset_img Capture05.png Modified CRM file %}


And you are done! You can update some other properties to customize the snow storm according to your preferences. Something you can do is increase the number and the size of the snow flakes. 

## Update for Unified Interface

We are in 2018 and the Unified Interface is here! so, we want the Christmas spirit to get there too!

To make this work on the new UI you need to repeat step #2 in the Unified Interface and find a different victim. 

You can choose literally any file under the WebResources folder, but it seems appropriate to use the mscommon.js file. 

{% asset_img Capture06.png Developer Tools Web Resources %}


Add the same piece of code added in step #4

```js
// snow script. remove after Christmas
var script = document.createElement('script');
script.src = "/webresources/new_snow.js";
document.head.appendChild(script);
// end snow script
```

The modified file will look something like...

{% asset_img Capture07.png Modified CRM file %}

Last but not least, there is a small correction required on the **new_snow.js** file. For some reason, in the Unified Interface, the window.onload event is not triggered the way the script expects and the line highlighted below just doesn't work.

So, find the autostart line and comment it all out. 

Add instead the following code:
``` js
  window.setTimeout(doStart, 2000);
```

{% asset_img Capture08.png Modified CRM file %}

And voila! The snow effect works nicely on the Unified Interface too!

{% asset_img Capture09.gif Snow Effect Capture %}


Happy Hacking Christmas!

This post was first published on http://blog.crisfervil.com/2018/12/06/Snow-effect-in-Dynamics-365/