## Strong reference cycles with closures - WIP

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

