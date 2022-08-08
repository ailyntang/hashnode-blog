## Background thread crashes and locks

When multiple background threads access the same data asynchronously, there is a chance a crash can happen.

Why?

Because the underlying data can change unexpectedly, and when that happens, it will cause a crash.

**Crash example: 2 threads accessing the same array**

Imagine - thread 1 accesses an array with 10 elements:
* Thread 1's purpose is to cycle through each element of the array, and perform an action
* It is up to element 4
* The scheduler tells thread 1 to pause what it's doing, and the scheduler gives thread 2 the clear to start doing what thread 2 needs to do


Thread 2 accesses the SAME array with 10 elements
* Thread 2's purpose is to delete all the elements in the array
* Thread 2 does that
* Thread 2 tells the scheduler it is finished
* The scheduler goes back to thread 1 and says - unpause, continue with what you were doing
* Thread 1 is expecting an array with 10 elements, it tries to access element 4 - but element 4 doesn't exist anymore. App crashes.

To prevent this crash, we can:
1. Architect the code properly, so that the app isn't put into a situation where multiple threads are likely / able to access the same data
    * We should always do this. This is best practice.
2. Proactively introduce a `lock` on the data
    * Sometimes we need to do this. However, we should always ensure we have done #1 first. 
    * A codebase with a lot of `lock`s is a bad smell - likely the code is poorly architected.


**What is a `lock`**

You apply a lock to the underlying data.
That way, only the element with a key can access the data. No one else can.
`lock()` is a function that exists already in the Swift library.

In the example above, thread 1 will lock the data.
Then when thread 2 tries to access it, nothing will happen.
Thread 2 can tell the scheduler the data is locked (perhaps this happens automatically).

When the scheduler flips back to thread 1, thread 1 will unlock the data when it is finished performing its task.

Then when the scheduler flips back to thread 2, thread 2 will see the new array. No crash on thread 1.


**When to use a lock? Example: Banking databases**

Imagine there is one banking database that holds all its users' account balances.
* It's very like that many users will try and access that database and try and UPDATE their data. E.g. everyone is trying to buy coffee in the morning.
* Joanne buys a coffee. She sends off a `PATCH api/updateBankAccount/183764?amount=-500` request to the database. Her bank balance should decrease from $100 to $95.
*At the same time, Caroline buys a coffee. She sends off `PATCH api/updateBankAccount/6235128?amount=-300`, which hits the same bank balance database. Caroline's balance should decrease from $20 to $17.

The database needs to process BOTH requests.
But if the two requests hit the database at the SAME time, then it's possible that Joanne's account is updated to $95, but then Caroline's account is updated slightly later to $17 and the database version on Caroline's thread still has Joanne's account balance at $100.

A lock is really vital so that only ONE request is processed at a time, to preserve the integrity of the data.