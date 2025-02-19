---
title: "c#: async awaits - under the hood"
datePublished: Mon May 20 2024 09:26:00 GMT+0000 (Coordinated Universal Time)
cuid: clwerh8bd00000amjbn0l6rc2
slug: c-async-awaits-under-the-hood
tags: csharp, interview-prep

---

*Updated: Feb 2025*

Key points about `async awaits`

* There are two relevant use cases for `async awaits`
    
    1. **I/O-bound**: when you are waiting for something, like reading from a database, writing to a database
        
    2. **CPU-bound**: when you don't want to freeze your UI (at work, this use case is not relevant for us)
        
* Benefits
    
    * Better resource management. Once you use `await`, the system deallocates the thread and allows something else to use that resource. This means you can scale to 1000 people making toast in the morning, even though `async await` is NOT the same as parallel programming
        
    * If you use `async`, the true benefit only comes with the `await` for the resource management
        
* Pitfalls
    
    * Once you start.. you can’t stop ! *Everything* in your system ends up needing to be async, as you bubble up the async. Mixing sync and async is not good practice.
        
* Alternatives
    
    * There aren’t really any alternatives
        

---

So the concept of `async awaits` is just the standard asynchronous vs synchronous topic. I've written about that before for freeCodeCamp.

But, the C# under the hood state machine is new to me.

Some concepts I learnt

* When is it relevant to use `async awaits`?
    
* You don't need to immediately `await` the task
    
* `async await` generates a lot of code under the hood
    
* `async` allows your system to scale
    

---

### When is it relevant to use `async awaits`?

There are two relevant use cases for `async awaits`

1. **I/O-bound**: when you are waiting for something, like reading from a database, writing to a database
    
2. **CPU-bound**: when you don't want to freeze your UI (at work, this use case is not relevant for us)
    

---

### You don't need to immediately `await` the task

You can separate

* kicking off of the task, and
    
* using the result of the task (which takes some time before it's ready to be used)
    

At Empower we often need the result straight away, which is why we almost always use `var result = await CreamButterAndSugarAsync();`

Here's how I *think* you would separate the two, based on some YouTube clips

* But I get the feeling this is wrong
    
* I think you actually need to do something like `CreamButterAndSugarAsync().Start / .Run` or something like that.
    

```plaintext
private async Task<Batter> CreamButterAndSugarAsync()
{
   // some task that will take 8 minutes to finish
   // return Batter;
}

public async Task MakeBananaCake()
{
   var creamButterAndSugarTask = CreamButterAndSugarAsync();

   // Go do all the other things now, whilst the butter and sugar are creaming
   // This will take ages, and you don't need the result yet
   // e.g. measure out flour, separate the eggs
   
   // once you are ready to use the result of the task, then you call `await`
   var batter = await creamButterAndSugarTask;
   // add eggs to `batter`
}
```

---

### `async await` generates a lot of code under the hood

So use it only if you need to.

Every extra argument you pass into your async method creates another property to manage the state of.

So consider what you really need.

[https://sharplab.io/](https://sharplab.io/)

* This is a good ILSpy browser based tool
    
* I put in the below code to see the async state machine
    

```plaintext
using System;
using System.Threading.Tasks;

public class C {
    public async Task M() {        
        var result = await MakeCake();
        Console.WriteLine(result);
    }
    
    private async Task<string> MakeCake()
    {
        await Task.Delay(1000);
        return "Cake finished!";
    }
}
```

It comes back with a LOT of code.

---

### `async` allows your system to scale

So. You have some code that does this inside a `public class MakeCake`

```plaintext
var cakeBatter = await MakeCakeBatterAsync();
var pourIntoPan = await PourIntoPanAsync(cakeBatter);
var cookedCake = await PutInOvenAsync(cakeBatter, 200C, 20mins);
var icedCake = await CoolDownThenIceAsync(cookedCake, 20mins);
```

Here's the thing. These are all async functions, because you are getting them to do things that take time.

* BUT, they are run one after the other.
    
* You are waiting for the output of one, so that you can use it as the input to the next.
    
* So.. it looks kinda "synchronous" to me.
    

Why bother with the `async`?

* To optimise "resource allocation" (that may not be the right term). But basically, to make good use of your resources.
    
* If you made everything synchronous, for your particular `MakeCake` class, there would be no change when you ran it once, for one user.
    
    * The server holds on to the `MakeCake` class, and until everything is finished, it just sits `MakeCake` and holds its hand the entire time
        
* BUT, if you have multiple requests to `MakeCake`, all of a sudden, your resources are going to be tied up, and you won't be able to fulfil many cake orders when you are running them all synchronously.
    
* If `MakeCake` is made up of `async await` methods
    
    * Each time an async method is called, that "instance" of `MakeCake` can be "released" and used to go do something else, as the state machine holds the state of `MakeCake`
        
    * Until the async method is finished, the server knows there's nothing else it can do.
        
    * Whilst it waits for the method to finish, the server can go away and use that resource to do something else, like make another cake (instead of sitting there, holding the hand of the synchronous `MakeCake` )
        
* With async methods, you can exponentially increase the number of cakes you can make per second
    

So.. `async await` will not speed up the time it takes to make ONE cake.

But, it will mean that you can make 10,000 cakes at once, instead of only 5 cakes at once.