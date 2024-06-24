---
title: "Lazy loading, eager loading and explicit loading"
datePublished: Mon Jun 24 2024 03:41:39 GMT+0000 (Coordinated Universal Time)
cuid: clxsfl70k000208jr7lvy44l0
slug: lazy-loading-eager-loading-and-explicit-loading
tags: dotnet

---

### Lazy loading

* `virtual` properties are only loaded when they are required to be used
    
* This is controlled by an option that you set - you can make lazy loading the default way to load properties
    
* Has an `N+1` loading problem. If you have a `foreach` statement which needs to access a `virtual` property, then you will make a database call each time you iterate through that loop
    

### Eager loading

* Use the `.Include` and tell SQL to go and include that `virtual` property when the first database call is made
    
* It will populate that virtual property for ALL the elements in your database call
    
* Very easy, one SQL call upfront
    

### Explicit loading

* When you need more fine grained control than `eager loading`
    
* So let's say you don't need every single row to have its `virtual` property loaded. You only care about one particular row
    
* With explicit loading, you can specify that level of loading, just for that one row
    
* It will always have a second SQL call, where you specify what you want to be explicitly loaded, and how you want to do it