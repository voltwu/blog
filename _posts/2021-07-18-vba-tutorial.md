---
layout: post
title: "VBA Tutorial"
date: 2021-07-18 09:03:01+0800
categories:
  - VBA
tags:
navi-enable-vba: true
navi-name: 'VBA'
navi-order: a1
toc: false
toc_label: "TABLE OF CONTENTS"
toc_icon: "cog"
comments: false
description: 
excerpt: 
---
<!--navigation bar-->
<div class='navi-link-container'>
<a class='navi-link' href="">{{page.navi-name}}</a>
</div>
<!--navigation bar-->

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