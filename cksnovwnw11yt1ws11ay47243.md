## Refactoring summary

Martin Fowler: talk at Etsy
https://youtu.be/6wDoopbtEqk

* If you are refactoring correctly, every change is so small that it is not worth doing. The beauty is in the summary of all these small changes.
* Every commit is shippable. You can stop refactoring at any time. You never break any functionality when you refactor. If you break something, roll back to the last commit, do it again with a smaller change.
* You do not need to know what the end looks like when you begin refactoring. Greater refactors can happen at the end, when the cleanliness of your code allows you to see the opportunities that weren't visible at the start.
* The goal of refactoring is to make the code easier to manipulate, so that you can ship faster.
* Do not worry about performance when refactoring. Nowadays performance issues from refactoring is so rare. Refactor first. Then test for performance issues at the end and fix only if the issues exist. E.g. he prefers using  functions, called multiple times, rather than assigning a temporary variable. This has two issues: 
  * Functions can have side effects. If your function has side effects, then you will need to assign a temporary variable.
  * Performance hit. This is so rare nowadays though, that the vast majority of functions will not enter this zone. Never let this concern stop you from using inline functions. Instead, do the refactor, and then actually test for performance issues. If there are any performance issues, deal with them once they are apparent. Do not prevent yourself from refactoring correctly.
* Refactor vs rewrite: no hard and fast rule. Comes down to experience.
* Coding conventions: you must have them within your team. People cannot stick to their own personal conventions if the rest of the team does not use them nor agree. E.g. his personal convention: if a function returns an argument, that argument is called `result`. But Bob hates that.