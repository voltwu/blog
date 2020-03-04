---
layout: post
title: "EF Core Eager Loading"
date: 2020-03-03 01:01:01+0800
categories:
  - CSharp
tags:
  - EF
toc: false
toc_label: "TABLE OF CONTENTS"
toc_icon: "cog"
comments: true
description: Eager Loading means that data is loaded as part of initial query
excerpt: Eager Loading means that data is loaded as part of initial query
---
<span style="font-size: 0.75em;">
\>
<a href="/blog/csharp/2020/02/28/Entitiy-Framework-Tutorial/" style="cursor: pointer;text-decoration: none;">main</a>
\>
<a href="/blog/csharp/2020/02/28/EF-Core-Load-Related-Entities/" style="cursor: pointer;text-decoration: none;">query</a>
<span>

I supposed that you had known **Eager Loading** means that related entities are loaded as part of the initial query.

This project can be found in [Github][1]

use `Include` method or `ThenInclude` method to complete an eager loading. the `Include` method can specify what navigation property you want to include.  the `ThenInclude` can allow you to continue including further levels of related entities.

```c#
public async Task<List<Team>> loadTeamsAsync()
{
    using (var context = new SchoolContext())
    {
        return await context.teams.
            Include(t => t.players).
                ThenInclude(p => p.addresses).
            ToListAsync();
    }
}
```

>**Caution**:
>
>Since version 3.0.0(.net core 3.0.0), each Include will cause an additional JOIN to be added to SQL queries produced by relational providers, whereas previous versions generated additional SQL queries. This can significantly change the performance of your queries, for better or worse. In particular, LINQ queries with an exceedingly high number of Include operators may need to be broken down into multiple separate LINQ queries in order to avoid the cartesian explosion problem.

[1]: https://github.com/voltwu/C-Sharp-Console-Application-EF-Core-Example/tree/b2d33ad3f6f19e06b20afeb68218798c7f2f9f08


