---
layout: post
title: "Policy Based Authorization"
date: 2020-06-18 09:04:01+0800
categories:
  - CSharp-ASP.NET Core
tags:
  - .net core
navi-enable-csharpaspnetcore: true
navi-name: 'policy based authorization'
navi-order: 'a1-6-1-4'
toc: true
toc_label: "TABLE OF CONTENTS"
toc_icon: "cog"
comments: true
description: how to add custom policy, how to implements custom requirements, how to add custom policy, let's explore it together.
excerpt: how to add custom policy, how to implements custom requirements, how to add custom policy, let's explore it together.
---
<!--navigation bar-->
<div class='navi-link-container'>
  {% assign posts = site.posts|sort:'navi-order' %}
  {% for post in posts %}
    {% if post.navi-enable-csharpaspnetcore %}
        {% if post.navi-order == "a1" or 
              post.navi-order == "a1-6" or 
              post.navi-order == "a1-6-1" %}
            <a href="{{ site.baseurl }}{{ post.url }}" class='navi-link'>{{post.navi-name}}</a>
        {%endif%}
    {% endif %}
  {% endfor %}
<a class='navi-link' href="">{{page.navi-name}}</a>
</div>
<!--navigation bar-->

Underneath the previous cover, we mentioned [role-based authorization][1]. You may hear another term, which is known as *claim-based authorization*. The principle of *claim-based authorization* is that to check whether has target claims.

```c#
var userClaim = new List<Claim>()
{
    new Claim(ClaimTypes.Email,"test@gmail.com")
    //...
};
```
Add a policy, which will require an Email claim type existing.

```c#
services.AddAuthorization(config=> {
    config.AddPolicy("emailRequire",policyBuilder=> {
        policyBuilder.RequireClaim(ClaimTypes.Email);
    });
});
```

Add the policy against a controller or an action.

```c#
[authorize(Policy = "emailRequire")]
public Controller EmailRequireController() 
{
}
```

The above example illustrates a simple role-based authorization, which involves another knowledge spot of this article, **Policy-Based Authorization**.

<!-- what's the policy based authorization, what's advantages it have? how do I implements it. -->


[1]: /blog/csharp-asp.net%20core/2020/06/15/role-based-authorization/#policy-based-role-checks
