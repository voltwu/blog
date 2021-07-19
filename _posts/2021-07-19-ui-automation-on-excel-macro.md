---
layout: post
title: "UI Automation On Excel Macro"
date: 2021-07-19 10:04:01+0800
categories:
  - VBA
tags:
navi-enable-vba: true
navi-name: 'UI Automation On Excel Macro'
navi-order: 'a1-1-1'
toc: true
toc_label: "TABLE OF CONTENTS"
toc_icon: "cog"
comments: true
description: a simple macro excel application with Microsoft UI Automation Client component.
excerpt: a simple macro excel application with Microsoft UI Automation Client component.
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

In this article, I am going to lead you to create a simple macro excel application with Microsoft UI Automation Client component step by step. As we already illustrated basic concepts at the [UI Automation On VBA][4], so here will focus on technical parts.

You need to create an Excel file and open its vba editor with striking `[ALT]`+`[F11]`. 

# Adding Reference

At the menu `Tools` -> `References...`, scroll down to find and refer `UIAutomationClient` component. 

<div class="imgcenter" markdown="1">
![Alt][5]*Adding UI AutomationClient reference*
</div>

# Creating CUIAutomation Object
```VBA
 Dim oAutomation As New CUIAutomation
```

# Searching Elements
UI Automation Client provides several available ways to find elements. You can use `inspecter.exe` as an assistive software, which can inspect details of UI elements(such as: **classname**, **automation id**, and **name**). 

`Scope` and `Condition` are required for searching elements. `Scope` specifies the scope of variant operations in UI Automation tree. `Condition` used in filtering when searching for elements in the UI Automation tree. Take a look at [TreeScope enumeration][1] and [IUIAutomationCondition interface][2] for more details.

**Find first child or descendant element**

```vba
' Creating CUIAutomation element
Dim oAutomation As New CUIAutomation

' find first child element with classname
' oAutomation.GetRootElement is the top-level element
Dim MyElement1 As UIAutomationClient.IUIAutomationElement
Set MyElement1 = oAutomation.GetRootElement.FindFirst(TreeScope_Children, UiAutomation.CreatePropertyCondition(UIAutomationClient.UIA_ClassNamePropertyId, "class name"))

' find first descendant element with automation id
Dim MyElement2 As UIAutomationClient.IUIAutomationElement
Set MyElement2 = MyElement1.FindFirst(TreeScope_Descendants, UiAutomation.CreatePropertyCondition(UIAutomationClient.UIA_AutomationIdPropertyId, "automation id"))

' find first parent element with name
Dim MyElement3 As UIAutomationClient.IUIAutomationElement
Set MyElement3 = MyElement2.FindFirst(TreeScope_Parent, UiAutomation.CreatePropertyCondition(UIAutomationClient.UIA_NamePropertyId, "name"))
``` 

**Find All elements**

```vba
' Creating CUIAutomation element
Dim oAutomation As New CUIAutomation

' find all elements with "button" as their's classname
Dim MyElement4 As UIAutomationClient.IUIAutomationElementArray
Set MyElement4 = MyElement3.FindAll(TreeScope_Children, UiAutomation.CreatePropertyCondition(UIAutomationClient.UIA_ClassNamePropertyId, "button"))
```

**Get Element's unique identifier**

```vba
' Get a unique identifier of an element
 Dim uniqueIdentifier As Variant
 uniqueIdentifier = MyElement3.GetRuntimeId()
```
The identifier is only guaranteed to be unique to the UI of the desktop on which it was generated. Identifiers can be reused over time.

Identifier may change in the future!!!

**Focus Element**

```vba
' make MyElement3 be focused on the screen window
MyElement3.SetFocus
```

# Retrieving Control Pattern

Assumpting that you had known the difference between Control Type and Control Pattern, If not, take a look at preivous article. Control Pattern provides client a way to manipulate ui elements(such as: click, input, or dropdown etc...)

Each UI Elements have at least one pattern, Take a look at [Control Pattern Identifiers][3] for more details.

**input**

```vba
' input text
Set oPattern = MyElement3.GetCurrentPattern(UIAutomationClient.UIA_LegacyIAccessiblePatternId)
oPattern.SetValue ("input text")
```

**click**
``` vba
' click a button
' btnElement is a button element
Set oPattern = btnElement.GetCurrentPattern(UIAutomationClient.UIA_InvokePatternId)
ePattern.Invoke
```

[1]: https://docs.microsoft.com/en-us/windows/win32/api/uiautomationclient/ne-uiautomationclient-treescope
[2]: https://docs.microsoft.com/en-us/windows/win32/api/uiautomationclient/nn-uiautomationclient-iuiautomationcondition
[3]: https://docs.microsoft.com/en-us/windows/win32/winauto/uiauto-controlpattern-ids
[4]: ../../../../vba/2021/07/18/ui-automation-on-vba/
[5]: /blog/public/img/2021-07-19-ui-automation-on-excel-macro.png

