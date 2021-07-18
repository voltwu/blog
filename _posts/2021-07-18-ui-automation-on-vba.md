---
layout: post
title: "UI Automation On VBA"
date: 2021-07-18 10:04:01+0800
categories:
  - VBA
tags:
navi-enable-vba: true
navi-name: 'UI Automation On VBA'
navi-order: 'a1-1'
toc: false
toc_label: "TABLE OF CONTENTS"
toc_icon: "cog"
comments: false
description: 
excerpt: 
---
<!--navigation bar-->
<div class='navi-link-container'>
  {% assign posts = site.posts|sort:'navi-order' %}
  {% for post in posts %}
    {% if post.navi-enable-vba %}
        {% assign number = page.navi-order | split: post.navi-order | size %}
        {% if number == 2 %}
            <a href="{{ site.baseurl }}{{ post.url }}" class='navi-link'>{{post.navi-name}}</a>
        {%endif%}
    {% endif %}
  {% endfor %}
<a class='navi-link' href="">{{page.navi-name}}</a>
</div>
<!--navigation bar-->
In this chapter, I am gonna illustrate how to use the Microsoft UI Automation component. Microsoft UI Automation is a new accessibility framework for Microsoft Windows, available on all operating systems that support Window Presentation Foundation(WPF).

It's easy to build up robustness and assistive programs with Microsoft UI Automation. Several concepts need to be familiar before this article. Microsoft UI Automation contains four main components, as shown in the following table.

|Component   |DLL   |Description   |
|---|---|---|
|Provider API  |UIAutomationProvider.dll and UIAutomationTypes.dll   |A set of interface definitions that are implemented by UI Automation providers, objects that provide information about UI elements and respond to programmatic input.   |
|Client API   |UIAutomationClient.dll and UIAutomationTypes.dll   |A set of types for managed code that enables UI Automation client applications to obtain information about the UI and to send input to controls.   |
|Core   |UiAutomationCore.dll   |The underlying code (sometimes called the UI Automation core) that handles communication between providers and clients.   |
|ClientsideProviders   |UIAutomationClientsideProviders.dll   |A set of UI Automation providers for standard legacy controls. (WPF controls have native support for UI Automation.) This support is automatically available to client applications.   |

The following dialogue represented the relationship between **Automation Provider**, **Automation Client**, and **Automation Core**.

<div class="imgcenter" markdown="1">
![Alt][1]*Relationship Between Provider, Client And Core*
</div>

Basically, for developers, there are two perspectives: to create support for custom controls (using the provider API), and creating applications that use the UI Automation core to communicate with UI elements (using the client API). These chapter's emphasis is to illustrate how to use UI Automation Client. For understand better about Microsoft UI automation element tree structure, need to acquaintance with **Control Patterns** and **Control Type**.

[AutomationElement][2] objects expose common properties of the UI elements they represent. One of these properties is the control type, which defines its basic appearance and functionality as a single recognizable entity: for example, a button or check box.

In addition, elements expose control patterns that provide properties specific to their control types. Control patterns also expose methods that enable clients to get further information about the element and to provide input.

UI Automation Client also provides a mechanism that enables a client to gather information through **events**. You can register specific event notifications, and the element's properties and control patterns will pass into their handlers when the event raise.


<!--items-->
<div>
<span style="color: green;">In this article:</span>
<ul>
  {% assign posts =site.posts | sort: 'navi-order' %}
  {% for post in posts %}
    {% if post.navi-enable-vba %}
      {% assign splitorder = page.navi-order | append: "-" %}
      {% assign splitnum = post.navi-order | split: splitorder %}
      {% assign stagesnumber = splitnum | split: "-" | size %}
      {% assign beginstagesnumber = splitnum | split: "a" | size %}
      {% if stagesnumber == 1 and beginstagesnumber == 1 %}
                <li><a href="{{ site.baseurl }}{{ post.url }}" class="item-link">{{post.title}}</a></li>
      {% endif %}
    {% endif %}
  {% endfor %}
</ul>
</div>
<!--items-->


[1]: /blog/public/img/2021-07-18-ui-automation-on-vba-a.png
[2]: https://docs.microsoft.com/en-us/dotnet/api/system.windows.automation.automationelement?view=net-5.0