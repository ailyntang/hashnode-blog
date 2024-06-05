---
title: "Intro to Web API"
datePublished: Tue Jun 04 2024 03:22:04 GMT+0000 (Coordinated Universal Time)
cuid: clwzu2yys00040aku8yohbl17
slug: intro-to-web-api
tags: dotnet

---

[https://www.youtube.com/watch?v=87oOF9Ve-KA&t=2199s](https://www.youtube.com/watch?v=87oOF9Ve-KA&t=2199s)

What I learnt

* There are two types of web APIs
    
    * Minimal API
        
    * API using Controllers (not sure what this is called - `full`? normal?)
        

These APIs serve users. The users aren't necessarily humans.

* Users can be another application
    
* The API is a User Interface
    
* A UI doesn't need to have buttons etc. If you expect visual things to interact with, then that's a GUI (graphical user interface)
    
* But because APIs serve users, they have a UI. And because the users can be non-human, there is no GUI required
    

Minimal APIs are really great for microservices

Microservices typically have 1-3 endpoints.

* So the minimal API construct keeps the code in `Program.cs` very lean and easy to read, without needing all the `Controller` code that a full API comes with
    

The user has no idea whether you are using a full API or minimal API behind the scenes.

* It's just personal preference