## Strong reference cycles with closures

# General ARC concepts

Key things to remember
* Only reference types (e.g. class) have a ARC
* The instance of a class is what gets a reference count, not the UI element inside the class
* If structs are referred to inside a closure, then the struct is copied
* If reference types are referred to inside a closure, then that increases the count of that instance, and can lead to a retain cycle
* "capturing"  refers to something outside the closure being referenced inside the closure. If `someClass` is passed into the closure, it isn't getting "captured". So the use of `someClass` does not add a count to `someClass`'s ARC.


The variables, constants, UI elements, closures etc inside the class do NOT have their own reference count.

So whenever something has a strong reference to a button, or a closure, it's the class that has an increased count. Not the button or the closure.

---------

# Example 1: NO GOOD - outer closure has a strong reference to `self`

```
// Outer closure
action = {

   // some actions here that do not reference `self`, so no need for a capture list with `[weak self]

   // Inner closure
   View.action = { [weak self]
       self?.dothing()
    }
}
```
The nested inner closure will look to the outer closure for a reference to self.
It's still a reference to the main view controller. But the reference itself is from the outer closure.

Example 1: because the outer closure has a strong reference to `self`, then the inner closure will also have a strong reference to self.

Perhaps the outer closure isn't referencing self at all, because it just does some print statements.
So if wasn't for the nested inner closure, there would be no retain cycle.

However the inner closure references `self`, and it will look to the outer closure to create that reference to `self` on its behalf.

So, even though the inner closure has a capture list with `[weak self]`, this weakly captures the relationship between the inner and outer closure.

Then the outer closure still needs to propagate the `self` reference, and so it will do this as `strong`.


-------------

# // Example 2: BE CAREFUL - can still have retain cycle because of `guard let`

```
// Outer closure
action = {  [weak self] in
   guard let self = self else { return }

   View.action = {
       self.dothing() // Strong reference to self
    }
}
```
Swift has two options of `self` to choose from in the inner closure.
1. The `let self`, which is a strong reference to self.
2. The `[weak self]`

Swift will default to choosing the closest definition, which is #1. 
Thus the inner closure is has a strong reference to self.
Which means the ARC of the Main View Controller is increased by 1, and will create a retain cycle.

There are two ways to avoid this retain cycle:
1. Use something like `guard let cake = self else { return }`. This way, Swift can't automatically choose the strong reference in the inner closure. Instead, swift will choose the weak self, and will prompt you to do `self?.doThing()`
2. In the inner closure, implement a capture list with `[weak self]`. ie.

```
action = {  [weak self] in
   guard let self = self else { return }

   View.action = { [weak self] in
       self?.dothing() // now this is a weak reference
    }
}
```

------------

# Example 3: guard let cake = self

Now that we are explicitly giving the strong reference to `self` a different name, `cake`, then it is really clear that the nested closure is not using `cake`. 

So no retain cycle there.

If the nested closure used `cake.doThing()`, then there would be a retain cycle, and we should have `view.action { [weak cake] in cake?.doThing() }`

```
// Outer closure
action = {  [weak self] in
   guard let cake = self else { return }


   View.action = {
       self.dothing() // this is referencing the weak version of self, so no retain cycle
    }
}
```

