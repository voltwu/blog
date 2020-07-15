---
layout: post
title: "Warm-up: Build a .net core console app"
date: 2020-07-13 09:03:01+0800
categories:
  - CSharp-ASP.NET Core
tags:
  - .net core
  - Container Technique
navi-enable-csharpaspnetcore: true
navi-name: 'Warm-up: Build a .net core console app'
navi-order: 'a1-9-1-1'
toc: false
toc_label: "TABLE OF CONTENTS"
toc_icon: "cog"
comments: true
description: Docker is a container technique, which is lightweight and convenient, and it has become a massively popular technology. What are you waiting for, let’s explore it together! This article will show you a simple, which you can learn how to build a .net Core Console application in Docker step by step.
excerpt: Docker is a container technique, which is lightweight and convenient, and it has become a massively popular technology. What are you waiting for, let’s explore it together! This article will show you a simple, which you can learn how to build a .net Core Console application in Docker step by step.
---
<!--navigation bar-->
<div class='navi-link-container'>
  {% assign posts = site.posts|sort:'navi-order' %}
  {% for post in posts %}
    {% if post.navi-enable-csharpaspnetcore %}
        {% if post.navi-order == "a1" or
            post.navi-order == "a1-9" or
            post.navi-order == "a1-9-1"
            %}
            <a href="{{ site.baseurl }}{{ post.url }}" class='navi-link'>{{post.navi-name}}</a>
        {%endif%}
    {% endif %}
  {% endfor %}
<a class='navi-link' href="">{{page.navi-name}}</a>
</div>
<!--navigation bar-->

Docker is a container technique, which is lightweight and convenient, and it has become a massively popular technology. What are you waiting for, let's explore it together!

This article will show you a simple, which you can learn how to build a .net Core Console application in Docker step by step.

# Prerequisites
* Familiar with Docker
* Familiar with .NET CORE Console Application

# Build and Publish a console App
You can choose different editors, like VSCode, Visual Studio, etc. This article builds an App through the command-line interface. Make sure you have [.NET CORE SDK][1] installed.

Use the following snippet to create a .NET Core app. 
```
dotnet new console -o MyApp
```

Get into `MyApp` folder. then run your App.
```
[root]# cd MyApp/
[root]# dotnet run
Hello World!
```

Publish your App.
```
[root]# dotnet publish -c release
Microsoft (R) Build Engine version 16.6.0+5ff7b0c9e for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  All projects are up-to-date for restore.
  MyApp -> /root/user/MyApp/bin/release/netcoreapp3.1/MyApp.dll
  MyApp -> /root/user/MyApp/bin/release/netcoreapp3.1/publish/
```

Get into your released folder, then run your `.dll` file up. 
```
[root]# cd bin/release/netcoreapp3.1/publish/
[root]# ll
total 112
-rwxr-xr-x 1 root root 90712 Jul 13 21:42 MyApp
-rw-r--r-- 1 root root   385 Jul 13 21:42 MyApp.deps.json
-rw-r--r-- 1 root root  4608 Jul 13 21:42 MyApp.dll
-rw-r--r-- 1 root root   620 Jul 13 21:42 MyApp.pdb
-rw-r--r-- 1 root root   146 Jul 13 21:42 MyApp.runtimeconfig.json

[root]# dotnet MyApp.dll
Hello World!
```

# Containerize
Create a `Dockerfile` with the following snippet to containerize your released App.
```
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1

COPY bin/release/netcoreapp3.1/publish/ App/
WORKDIR /App

ENTRYPOINT ["dotnet", "MyApp.dll"]
```

Put the above Dockerfile under MyApp directory.
```
[root]# ls
total 20
drwxr-xr-x 4 root root 4096 Jul 13 21:42 bin
-rw-r--r-- 1 root root  141 Jul 13 21:56 Dockerfile
-rw-r--r-- 1 root root  178 Jul 13 21:41 MyApp.csproj
drwxr-xr-x 4 root root 4096 Jul 13 21:42 obj
-rw-r--r-- 1 root root  187 Jul 13 21:41 Program.cs
```

Build an image.
```
[root]# docker build -t hello-world:1.0 .
Sending build context to Docker daemon  700.9kB
Step 1/4 : FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
 ---> 014a41b1f39a
Step 2/4 : COPY bin/release/netcoreapp3.1/publish/ App/
 ---> fbfccbc52704
Step 3/4 : WORKDIR /App
 ---> Running in 34f680e442c9
Removing intermediate container 34f680e442c9
 ---> 89134e5935e7
Step 4/4 : ENTRYPOINT ["dotnet", "MyApp.dll"]
 ---> Running in c1874d0701b5
Removing intermediate container c1874d0701b5
 ---> ed920239370c
Successfully built ed920239370c
Successfully tagged hello-world:1.0
```

Run your image as container.
```
[root]# docker run --name hello-world hello-world:1.0
Hello World!
```
Now, your container is running up successfully. If you want to extend more, check the [Docker][2]. 

The above process only containerized your released App. As the Docker is a container, so you can do the same thing as you are in a real operating system. You can build, publish, release your App in a single Docker container.

The following Dockerfile shows you how to complete all operations(build, publish, release) in a Docker container.

```
FROM mcr.microsoft.com/dotnet/core/runtime:3.1-buster-slim AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["ConsoleApp1/ConsoleApp1.csproj", "ConsoleApp1/"]
RUN dotnet restore "ConsoleApp1/ConsoleApp1.csproj"
COPY . .
WORKDIR "/src/ConsoleApp1"
RUN dotnet build "ConsoleApp1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ConsoleApp1.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ConsoleApp1.dll"]
```
The above Dockerfile comes from Visual Studio Docker Tools, and the Dockerfile related to multi-stage features, which was introduced in docker 17.05. Check these articles [How Visual Studio builds containerized apps][3], and [Use multi-stage builds][4] to learn more about Docker and Visual Studio.

```
[root]# dir
[root]# cd ..
[root]# docker run -f ConsoleApp1/Dockerfile ConsoleApp1/
Hello World!
```
Use the `-f` to specify the Dockerfile explicitly.

# Conclusion
Through this tutorial, you've learned how to deploy NET CORE Console in Docker. Take a look review.

1. Docker is a container, which provides all the necessary packages you need. You can configure your App to be built, run, release in a single Dockerfile(use the multistage feature).
2. Visual Studio Docker Support Tools takes the advantages of the Docker's multi-stage features.
3. If you want to learn more about Docker, see [Docker Tutorial][2].


[1]: https://dotnet.microsoft.com/download
[2]: /blog/docker/2020/07/08/docker-tutorial/
[3]: https://docs.microsoft.com/en-us/visualstudio/containers/container-build?view=vs-2019
[4]: https://docs.docker.com/develop/develop-images/multistage-build/

