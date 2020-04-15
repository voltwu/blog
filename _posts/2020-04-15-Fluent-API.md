---
layout: post
title: "Fluent API"
date: 2020-04-15 10:01:01+0800
categories:
  - CSharp-EF
tags:
  - ORM
toc: false
toc_label: "TABLE OF CONTENTS"
toc_icon: "cog"
comments: true
navi-enable-ef: true
navi-name: ""
navi-order: 'a1-4-1'
description: the fluent api is another data annotation implemention which applied more functions.
excerpt: the fluent api is another data annotation implemention which applied more functions.
---
<!--navigation bar-->
<div class='navi-link-container'>
  {% assign posts = site.posts|sort:'navi-order' %}
  {% for post in posts %}
    {% if post.navi-enable-ef %}
        {% if post.navi-order == "a1" or 
              post.navi-order == "a1-4"%}
            <a href="{{ site.baseurl }}{{ post.url }}" class='navi-link'>{{post.navi-name}}</a>
        {%endif%}
    {% endif %}
  {% endfor %}
<a class='navi-link'></a></div>
<!--navigation bar-->

In Entity Framework Core, you can override the `DbContext.ModelBuilder` method of `DbContext` to use the Fluent API functionality.

```c#
class SchoolContext : DbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("Server=_server;Database=_database;Trusted_Connection=True;");
    }
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Student>(entity => {
            entity.ToTable("student","dbo");
            entity.HasKey(stu=>stu.Id);
            entity.Property(e => e.Id).
                HasColumnType("int").
                HasColumnName("id").
                IsRequired();
            entity.Property(e => e.name).
                HasMaxLength(128).
                HasColumnType("nvarchar").
                HasColumnName("name");
            entity.Property(e => e.add_time).
                HasColumnType("datetime").
                HasColumnName("addtime").
                HasDefaultValueSql("getdate()").
                ValueGeneratedOnAdd();
            entity.Property(e => e.last_modify_time).
                HasColumnType("datetime").
                HasColumnName("lastmodifytime").
                HasDefaultValueSql("getdate()").
                ValueGeneratedOnAddOrUpdate();
        });
    }
}
public class Student
{
    public int Id { set; get; }
    public string name { set; get; }
    public DateTime add_time { set; get; }
    public DateTime last_modify_time { set; get; }
}
```
Fluent API provides methods for configuring variable aspects:
1. Model Configuration
2. Entity Configuration
3. Property Configuration

**Model Configuration**
You can configure on the `ModelBuilder` instance such as the default scheme, functions, ignore given entities. 

The `ModelBuilder` instance includes a lot of methods that used to break the default convention.
```c#
modelBuilder.Ignore<Student>();
modelBuilder.HasDefaultSchema("dbo");
modelBuilder.HasDbFunction(typeof(SchoolContext).GetMethod("myCustomFunction"));
//...
//other configurations
```

**Entity Configuration**
configure entity to table and relationship mapping such as the Primary key, Foreign Key, Index, Table etc...

```c#
modelBuilder.Entity<Student>(entity => {
    entity.ToTable("student","dbo");
    entity.HasKey(stu=>stu.Id);
    entity.HasIndex(stu=>stu.gender);
});
public class Student
{
    public int Id { set; get; }
    public string name { set; get; }
    public int gender { set; get; }
    public DateTime add_time { set; get; }
    public DateTime last_modify_time { set; get; }
}
```
**Property Configuration**
configure property to column mapping such as column name, column type, default value, Foreign key, concurrency check, etc...
```c#
entity.Property(e => e.add_time).
    HasColumnType("datetime").
    HasColumnName("addtime").
    HasDefaultValueSql("getdate()").
    ValueGeneratedOnAdd();
```

