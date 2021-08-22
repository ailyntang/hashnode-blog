## Refactoring summary

Martin Fowler: talk at Etsy summary

* When you refactor, every change is so small that it is not worth doing. The beauty is in the summary of all these small changes.
* Every commit is shippable. You can stop refactoring at any time.
* You do not need to know what the end looks like when you begin refactoring.
* The goal of refactoring is to make the code easier to manipulate, so that you can ship faster.
* Do not worry about performance when refactoring. E.g. he prefers using  functions, called multiple times, rather than assigning a temporary variable. This has two issues: 
  * Functions can have side effects. If your function has side effects, then you will need to assign a temporary variable.
  * Performance hit. This is so rare nowadays though, that the vast majority of functions will not enter this zone. Never let this concern stop you from using inline functions. Instead, do the refactor, and then actually test for performance issues. If there are any performance issues, deal with them once they are apparent. Do not prevent yourself from refactoring correctly.