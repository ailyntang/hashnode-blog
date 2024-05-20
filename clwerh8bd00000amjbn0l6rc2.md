---
title: "c#: async awaits - under the hood"
datePublished: Mon May 20 2024 09:26:00 GMT+0000 (Coordinated Universal Time)
cuid: clwerh8bd00000amjbn0l6rc2
slug: c-async-awaits-under-the-hood
tags: csharp

---

So the concept of `async awaits` is just the standard asynchronous vs synchronous topic. I've written about that before for freeCodeCamp.

But, the C# under the hood state machine is new to me.

Some concepts I learnt.

### You don't need to immediately `await` the task

You can separate

* kicking off of the task, and
    
* using the result of the task (which takes some time before it's ready to be used)
    

At Empower we often need the result straight away, which is why we almost always use `var result = await CreamButterAndSugarAsync();`

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