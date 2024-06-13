---
title: "ASP.net core - custom middleware"
datePublished: Thu Jun 13 2024 05:55:26 GMT+0000 (Coordinated Universal Time)
cuid: clxcuivnq000w0al34lgh2gbk
slug: aspnet-core-custom-middleware
tags: aspnet-core

---

Main takeaway

* if something seems like it is magically happening, or breakpoints are never hit in your code..
    
* .. CHECK THE MIDDLEWARE! There may be middleware executing that has intercepted the request before it ever gets to your code.
    

---

Using the built in middleware is hard to see when stuff actually triggers

* it's all a bit like magic
    
* You put breakpoints inside the middleware components, but nothing ever seems to trigger. And your http request is fulfilled, without any breakpoints triggering
    

So, to get a real feel for middleware, I implemented some custom middleware.

* Now you can see that this is triggered once you make a request
    
* ie. once I navigate to `localhost:xxxx/weatherforecast`
    

```plaintext

app.Use(async (context, next) =>
{
    Console.WriteLine("Inside middleware 1, before next");
    await next(context);
    Console.WriteLine("Inside middleware 1, after next");
});

app.Use(async (context, next) =>
{
    Console.WriteLine("Inside middleware 2, before next");
    await next(context);
    Console.WriteLine("Inside middleware 2, after next");
});

app.Use(async (context, next) =>
{
    Console.WriteLine("Inside middleware 3, before next");
    await next(context);
    Console.WriteLine("Inside middleware 3, after next");
});
```

And the output is as expected as well.

Note that the `await next(context);` really awaits until that next context is COMPLETELY finished

* So that's why the request goes completely through the pipeline, and then returns one by one
    

```plaintext
Inside middleware 1, before next
Inside middleware 2, before next
Inside middleware 3, before next
Inside middleware 3, after next
Inside middleware 2, after next
Inside middleware 1, after next
warn: Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionMiddleware[3]
      Failed to determine the https port for redirect.
Inside middleware 1, before next
Inside middleware 2, before next
Inside middleware 3, before next
Inside middleware 3, after next
Inside middleware 2, after next
Inside middleware 1, after next
```

Examples of when to use custom middleware

* Timer
    
    * In our app, we use it to start a timer on the request, to see how long it will take for this request to complete
        
    * So this is one of the first middleware components in the pipeline
        
* Load balancing
    
    * Intercept the requests, and then based on the request, send that request to specific servers
        
* Silly reason:
    
    * maybe custom exception middleware where you want to be sent an email every time an exception happens
        

Once you get the feel for custom middleware, I get the feeling it can be overused for `everything`. Like, it becomes your favourite hammer in the toolbox.