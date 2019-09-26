---
layout: post
title: "Redirect With Web.config"
date: 2019-09-26
tags: [Azure, cloud, redirect]
---


### Target
There are two websites, one for desktop and another lite version for mobile devices.
To switch between these two websites seamlessly, I decide to use redirect action in the web.config file, which can avoid rendering content before redirect.

<br/>

### Plan
#### Desktop Website Auto Redirect
Desktop Website only auto redirect to the Mobile Website when:
1. The device is a **mobile** device <br/>
**AND**
2. Resources is **not** requested by the mobile website

Translate it to ASP.NET


~~~markdown
`<system.webServer>

    <rewrite>
      <rules>

      <rule name="Auto Redirect" patternSyntax="ECMAScript" stopProcessing="true">

                <match url=".*" ignoreCase="true"  />

                <conditions logicalGrouping="MatchAll" trackAllCaptures="true">
                    <add input="{HTTP_USER_AGENT}" pattern="midp|mobile|phone" />
                    <add input="{HTTP_REFERER}" negate="true" pattern="^(.*)your-mobile-site(.*)" />         
                </conditions>
                <action type="Redirect" url="https://your-mobile-site" appendQueryString="false" redirectType="Permanent" />

      </rule>

      </rules>
    </rewrite>

</system.webServer>`

~~~

#### Mobile Website
No changes need to be made.

<br/>

### Challenge 1:
Cannot detect HTTP_REFERER correctly
### Reason:
Only https websites can be detected at HTTP_REFERER. <br/>
>_A user agent MUST NOT send a Referer header field in an unsecured HTTP request if the referring page was received with a secure protocol._

Check at this [link](https://tools.ietf.org/html/rfc7231#section-5.5.2)

### Solution:
Enable https only on Azure setting for Desktop Website

<br/>

### Challenge 2:
Empty Page shown with CORB warning when switching back to desktop website form mobile site.
### Solution:
Avoid auto redirect for all the static files


~~~markdown
`<system.webServer>

    <rewrite>
      <rules>

      <rule name="Auto Redirect" patternSyntax="ECMAScript" stopProcessing="true">

                <match url=".*" ignoreCase="true"  />

                <conditions logicalGrouping="MatchAll" trackAllCaptures="true">
                    <add input="{HTTP_USER_AGENT}" pattern="midp|mobile|phone" />
                    <add input="{HTTP_REFERER}" negate="true" pattern="^(.*)your-mobile-site(.*)" />
                    <add input="{REQUEST_URI}" negate="true" pattern="^/img/your-image.png$" ignoreCase="true" />
                    <add input="{REQUEST_URI}" negate="true" pattern="^/static/js/your-js-chunk.chunk.js$" ignoreCase="true" />
                    <add input="{REQUEST_URI}" negate="true" pattern="^/static/css/your-css-chunk.chunk.css$" ignoreCase="true" />            
                </conditions>
                <action type="Redirect" url="https://your-mobile-site" appendQueryString="false" redirectType="Permanent" />

      </rule>

      </rules>
    </rewrite>

</system.webServer>`

~~~
<br/>

### Final Thoughts
The method is easy to understand, but need some foundation knowledge of HTTP and .NET.
I got stuck on this redirect thing for quite a while. For me, this is a lesson of basic. :)
