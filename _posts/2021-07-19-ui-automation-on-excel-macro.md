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

In this article, I am going to lead you to create a simple macro excel application with the Microsoft UI Automation Client component step by step. As we already illustrated basic concepts at the [UI Automation On VBA][4], so here will focus on technical parts.


I wrote a snippet of Microsoft UI Automation at [UI Automation Example][6], which may be assistive for you.

You need to create an Excel file and open its VBA editor with striking `[ALT]`+`[F11]`. 

# Adding Reference

At the menu `Tools` -> `References...`, scroll down to find and refer `UIAutomationClient` component. 

<div class="imgcenter" markdown="1">
![Alt][5]*Adding UI AutomationClient reference*
</div>

# Creating CUIAutomation Object
<pre>
 <vb-keywords>Dim</vb-keywords> oAutomation <vb-keywords>As</vb-keywords> <vb-keywords>New</vb-keywords> CUIAutomation
</pre>

# Searching Elements
UI Automation Client provides several available ways to find elements. You can use `inspecter.exe` as an assistive software, which can inspect details of UI elements(such as: **classname**, **automation id**, and **name**). 

`Scope` and `Condition` are required for searching elements. `Scope` specifies the scope of variant operations in UI Automation tree. `Condition` used in filtering when searching for elements in the UI Automation tree. Take a look at [TreeScope enumeration][1] and [IUIAutomationCondition interface][2] for more details.

**Find first child or descendant element**

<pre>
<vb-commets>' Creating CUIAutomation element</vb-commets>
<vb-keywords>Dim</vb-keywords> oAutomation <vb-keywords>As New</vb-keywords> CUIAutomation

<vb-commets>' find first child element with classname</vb-commets>
<vb-commets>' oAutomation.GetRootElement is the top-level element</vb-commets>
<vb-keywords>Dim</vb-keywords> MyElement1 <vb-keywords>As</vb-keywords> UIAutomationClient.IUIAutomationElement
<vb-keywords>Set</vb-keywords> MyElement1 = oAutomation.GetRootElement.FindFirst(TreeScope_Children, UiAutomation.CreatePropertyCondition(UIAutomationClient.UIA_ClassNamePropertyId, "class name"))

<vb-commets>' find first descendant element with automation id</vb-commets>
<vb-keywords>Dim</vb-keywords> MyElement2 <vb-keywords>As</vb-keywords> UIAutomationClient.IUIAutomationElement
<vb-keywords>Set</vb-keywords> MyElement2 = MyElement1.FindFirst(TreeScope_Descendants, UiAutomation.CreatePropertyCondition(UIAutomationClient.UIA_AutomationIdPropertyId, "automation id"))

<vb-commets>' find first parent element with name</vb-commets>
<vb-keywords>Dim</vb-keywords> MyElement3 <vb-keywords>As</vb-keywords> UIAutomationClient.IUIAutomationElement
<vb-keywords>Set</vb-keywords> MyElement3 = MyElement2.FindFirst(TreeScope_Parent, UiAutomation.CreatePropertyCondition(UIAutomationClient.UIA_NamePropertyId, "name"))
</pre>

**Find All elements**

<pre>
<vb-commets>' Creating CUIAutomation element</vb-commets>
<vb-keywords>Dim</vb-keywords> oAutomation <vb-keywords>As</vb-keywords> <vb-keywords>New</vb-keywords> CUIAutomation

' find all elements with "button" as their's classname
<vb-keywords>Dim</vb-keywords> MyElement4 <vb-keywords>As</vb-keywords> UIAutomationClient.IUIAutomationElementArray
<vb-keywords>Set</vb-keywords> MyElement4 = MyElement3.FindAll(TreeScope_Children, UiAutomation.CreatePropertyCondition(UIAutomationClient.UIA_ClassNamePropertyId, "button"))
</pre>


**Get Element's unique identifier**

<pre>
<vb-commets>' Get a unique identifier of an element</vb-commets>
<vb-keywords>Dim</vb-keywords> uniqueIdentifier <vb-keywords>As</vb-keywords> Variant
 uniqueIdentifier = MyElement3.GetRuntimeId()
</pre>

The identifier is only guaranteed to be unique to the UI of the desktop on which it was generated. Identifiers can be reused over time.

<blockquote class="quote">
<b>NOTE:</b>  
Identifier may change in the future !!!
</blockquote>

**Focus Element**

<pre>
<vb-commets>' make MyElement3 be focused on the screen window</vb-commets>
MyElement3.SetFocus
</pre>

# Retrieving Control Pattern

Assumpting that you had known the difference between Control Type and Control Pattern, If not, take a look at the previous article. Control Pattern provides the  client a way to manipulate ui elements(such as: click, input, or dropdown, etc...)

Each UI Elements have at least one pattern, Take a look at [Control Pattern Identifiers][3] for more details.

**Input**

<pre>
<vb-commets>' input text</vb-commets>
<vb-keywords>Set</vb-keywords> oPattern = MyElement3.GetCurrentPattern(UIAutomationClient.UIA_LegacyIAccessiblePatternId)
oPattern.SetValue ("input text")
</pre>

**Click**
<pre>
<vb-commets>' click a button</vb-commets>
<vb-commets>' btnElement is a button element</vb-commets>
<vb-keywords>Set</vb-keywords> oPattern = btnElement.GetCurrentPattern(UIAutomationClient.UIA_InvokePatternId)
ePattern.Invoke
</pre>

# Conclusion
Above straightly illustrated the steps of creating an excel macro application with Microsoft UI Component.  The following reviews might help you to understand better.
* Adding `UIAutomationClient` in reference window
* Searching elements through `FindFirst` or `FindAll`
* Performing actions through `Control Pattern`


[1]: https://docs.microsoft.com/en-us/windows/win32/api/uiautomationclient/ne-uiautomationclient-treescope
[2]: https://docs.microsoft.com/en-us/windows/win32/api/uiautomationclient/nn-uiautomationclient-iuiautomationcondition
[3]: https://docs.microsoft.com/en-us/windows/win32/winauto/uiauto-controlpattern-ids
[4]: ../../../../vba/2021/07/18/ui-automation-on-vba/
[5]: /blog/public/img/2021-07-19-ui-automation-on-excel-macro.png
[6]: https://gist.github.com/voltwu/fe90b3e7dd001c11228597f0ac29ab19