## Strong reference cycles with closures - WIP

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

```
// Outer closure
action = {  [weak self] in
   guard let cake = self else { return }


   View.action = {
       self.dothing() // this is referencing the weak version of self, so no retain cycle
    }
}
```


--------
# General ARC concepts

Key things to remember
* Only reference types have a ARC
* 



--------
# Notes to delete 

Let's say you have `MainViewController`, with a button in it.
When you tap the button, it performs an action.

This action is a closure.


Some psuedocode:

```
class MainViewController: UIViewController {

   let title: String = "Ho ho ho"
   let button = UIButton()

   func updateTitleTo(_ newTitle: String) {
       title = newTitle
   }
}
```

Now when we tap the button:
```
button.tap {
   self.updateTitleTo("meep morp")
}
```

This closure has a strong reference to self.
So we introduce a `[weak self]` to avoid a retain cycle.

But note if we have `guard let self = self else { return }` then have a nested closure, the nested closure will have a strong reference to `self` by default.

So the nested closure also needs to have `[weak self]`.

----------
