---
layout: post
title: "Redirect With Web.config File"
date: 2019-09-26
tags: [Azure, cloud, redirect]
---


## Background
There are two websites, one for desktop and another lite version for mobile devices.
To switch between these two website seamlessly without rendering its content, I decide to use Redirect in the web.config file.


## Plan
### Desktop Site Auto Redirect

- Desktop Website only auto redirect to the Mobile Website when:
1. The device is a mobile device <br/>
**AND**
2. Resources is **not** requested by the mobile website

Translate it to ASP.NET


```markdown

`  
<system.webServer>

    <rewrite>
      <rules>

      <rule name="Auto Redirect" patternSyntax="ECMAScript" stopProcessing="true">

                <match url=".*" ignoreCase="true"  />

                <conditions logicalGrouping="MatchAll" trackAllCaptures="true">
                    <add input="{HTTP_USER_AGENT}" pattern="midp|mobile|phone" />
                    <add input="{HTTP_REFERER}" negate="true" pattern="^(.*)reserve-mobile(.*)" />         
                </conditions>
                <action type="Redirect" url="https://your-mobile-site" appendQueryString="false" redirectType="Permanent" />

      </rule>

      </rules>
    </rewrite>

  </system.webServer>`

```



## Challenge1: only https websites can be redirected


## Challenge2: disable auto redirect for all static files


## Final Thoughts:

## Resources:
