---
layout: post
title: "EF Core Load Related Entities"
date: 2020-02-28 10:01:01+0800
categories:
  - CSharp
tags:
  - EF
toc: false
toc_label: "Table of Contents"
toc_icon: "cog"
comments: false
description: EF Core load related entities
excerpt: Entity Framework Core allows you to use navigation propertities in your model to load related entities...
---
<span style="font-size: 0.75em;">
\>
<a href="/blog/csharp/2020/02/28/Entitiy-Framework-Tutorial/" style="cursor: pointer;text-decoration: none;">main</a>
<span>

Entity Framework Core allows you to use navigation properties in your model to load related entities. There are three common ORM patterns used to load related entities. They are Eager Loader, Explicit Loading, Lazying Loading. Those loading methods have different performance. I am going to dig into it in this article.

**Eager Loading** means that related entities are loaded from the database as part of the initial query.
**Explicit Loading** means that related entities are explicitly loaded from the database at a later time.
**Lazy Loading** means that related entities are transparently loaded from the database when the navigation property is accessed.  Lazy Loading was introduced in EF Core 2.1 as a new feature.

<div>
<span style="color: green;">In this article:</span>
<ul>
<li><a href="/blog/csharp/2020/03/03/EF-Core-Eager-Loading/" style="cursor: pointer;text-decoration: none;font-size: 0.85em;">Eager Loading</a></li>
<li><a href="/blog/csharp/2020/03/06/EF-Core-Explicit-Loading/" style="cursor: pointer;text-decoration: none;font-size: 0.85em;">Explicit Loading</a></li>
<li><a href="/blog/csharp/2020/03/19/EF-Core-Lazy-Loading/" style="cursor: pointer;text-decoration: none;font-size: 0.85em;">Lazy Loading</a></li>
<li><a href="/blog/csharp/2020/03/21/Analysis-of-advantage-and-disadvantage-of-Eager-Explicit-Lazy-Loading/" style="cursor: pointer;text-decoration: none;font-size: 0.85em;">How To Choose To Use Which One</a></li>
</ul>
</div>
