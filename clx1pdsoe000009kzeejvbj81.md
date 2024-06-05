---
title: "ASP.net Core pipeline"
datePublished: Wed Jun 05 2024 10:46:03 GMT+0000 (Coordinated Universal Time)
cuid: clx1pdsoe000009kzeejvbj81
slug: aspnet-core-pipeline
tags: aspnet-core

---

The pipeline takes an input, an http request

* It passes it through to the Kestrel server
    
* Then the request goes through middleware for processing
    
* Then it comes back out to the website for consumption
    

The middleware can do these things when it receives a request

* Do some code
    
    * What's an example?
        
* Decide to terminate
    
    * If this happens, then the request is passed back through to the last middleware
        
* Decide to pass the request on to the next middleware
    
    * It can pass along a context to the next middleware as well
        
    * After passing the request on, it can also execute some more code
        
        * What's an example of this?
            
        * Is this so that when the request passes back through, it can `pick up` this code?
            

* Each middleware is oblivious of the rest of the pipeline
    

**Order matters**

* These are individual units that you line up in order
    
* Order really matters, because the request is passed through and updated in that order
    
* Exception handling should be one of the first request delegates in the pipeline, so that it can handle any exceptions that occur along the rest of the pipeline
    

**Types of middleware / request delegates**

* Map, run, use
    
* Run is a terminal middleware. It does not pass the request on to the next delegate
    
* Use: has the concept of passing the request to the next delegate
    

**Terminology**

* Request delegate: this is the middleware block
    
* Terminal middleware
    
* Request processing pipeline
    

**Things I researched to get this understanding**

* Frank Liu's youtube channel
    
* [Microsoft](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/?view=aspnetcore-8.0)