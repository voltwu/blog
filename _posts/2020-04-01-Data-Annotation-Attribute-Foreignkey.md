---
layout: post
title: "Data Annotation Attribute ForeignKey"
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
navi-name: "foreignkey"
navi-order: "1-3-1"
description: denotes a property used as a foreign key in a relationship
excerpt: denotes a property used as a foreign key in a relationship
---
<!--navigation bar-->
<div class='navi-link-container'>
  {% assign posts = site.posts | sort: 'navi-order' | reverse  %}
  {% for post in posts %}
    {% if post.navi-enable-ef %}
        {% if post.navi-order == "1" or
              post.navi-order == "1-3"%}
            <a href="{{ site.baseurl }}{{ post.url }}" class='navi-link'>{{post.navi-name}}</a>
        {%endif%}
    {% endif %}
  {% endfor %}
</div>
<!--navigation bar-->


