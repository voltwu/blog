---
layout: post
title: "Data Annotations Attributes"
date: 2020-03-29 10:01:01+0800
categories:
  - CSharp-EF
tags:
  - ORM
toc: false
toc_label: "TABLE OF CONTENTS"
toc_icon: "cog"
comments: true
navi-enable-ef: true
navi-name: "annotations"
navi-order: "1-3"
description: Data annotations attributes help programmers easly specify database-related information.
excerpt: Data annotations attributes help programmers easly specify database-related information.
---
<!--navigation bar-->
<div class='navi-link-container'>
  {% for post in site.posts %}
    {% if post.navi-enable-ef and 
          post.navi-order == "1" %}
        <a href="{{ site.baseurl }}{{ post.url }}" class='navi-link'>{{post.navi-name}}</a>
    {% endif %}
  {% endfor %}
</div>
<!--navigation bar-->


Entity Framework Core and Entity Framework both provide attributes for help programmers conveniently specify database-related information.

Entities have their own conventions between entities and database tables. However, you can change it with extra data annotations attributes.

you can independently point out the name of the database, the name of the field, the name of the constraint, etc...  instead of using the default conventions.

These annotations are included in the `System.ComponentModel.DataAnnotations` namespace and the `System.ComponentModel.DataAnnotations.Schema` namespace. both are not framework depending namespaces. In other words, it is valid in EF 6 as well as valid in EF Core.

> Note: 
> `Annotations` only give you a subset of configuration options. and `Fluent API` provides full configuration options.

I am going to list out partial common attributes for reference purposes.

**`System.ComponentModel.DataAnnotations`** namespace

|Attribute   	|Description   	|
|---	|---	|
|Key   	|Denotes one or more properties that uniquely identify an entity   	|
|Required   	|Specifies that a data field value is required   	|
|MaxLength   	|Specifies the maximum length of array or string data allowed in a property   	|
|MinLength   	|Specifies the minimum length of array or string data allowed in a property   	|
|StringLength   	|Specifies the minimum and maximum length of characters that allowed in a data field   	|


**`System.ComponentModel.DataAnnotations.Schema`** namespace

|Attribute   	|Description   	|
|---	|---	|
|Table   	|Can be applied to an entity class to configure the corresponding table name and schema in the database   	|
|Column   	|Specifies the database column that a property is mapped to   	|
|ForeignKey   	|Denotes a property used as a foreign key in a relationship   	|
|NotMapped   	|Denotes a property or a class should be excluded from the database mapping   	|
|InverseProperty   	|Can be applied to a property to specify the inverse of a navigation property that represents the other end of the same relationship.   	|




<!--items-->
<div>
<span style="color: green;">In this article:</span>
<ul>
  {% assign posts =site.posts | sort: 'navi-order' %}
  {% for post in posts %}
    {% if post.navi-enable-ef %}
      {% if post.navi-order == "1-3-1" or
            post.navi-order == "1-3-2" or 
            post.navi-order == "1-3-3" or 
            post.navi-order == "1-3-4"%}
                <li><a href="{{ site.baseurl }}{{ post.url }}" class="item-link">{{post.title}}</a></li>
      {% endif %}
    {% endif %}
  {% endfor %}
</ul>
</div>
<!--items-->
