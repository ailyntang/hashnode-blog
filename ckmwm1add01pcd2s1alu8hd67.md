## Delegate is always nil

If this happens, make sure the following:

**Use a `final class` not a `struct`**

The thing holding the `weak var delegate` property needs to be an instance. Usually this is a view model.


**Make sure the view model is being held**

If the delegate is `nil` when you would expect it to be non-nil, then chances are it's being set, and then the view model is being de-initialised so then the delegate is nil again.

To check, put a `deinit { }` in the view model. Then you'll see if it's being cleaned up.

When you use breakpoints to trouble shoot, sometimes the delegate may work as expected. This is a signal that you are in this scenario.


**How to hold onto the view model?**
If you have this problem, then likely you are declaring your view model inside a function, like this:

```
func bakeCake() {
    let viewModel = CakeViewModel()
    // blah blah
   // the view model is de-initialised straight away
}
```

Instead, you need to declare the view model outside of the function. e.g.

```
final class CakeViewController: UIViewController {

   private var cakeViewModel: CakeViewModel?

    func bakeCake() {
        cakeViewModel = CakeViewModel()
        // now the view model will hang around until the view controller is deinitialised, which is what you want
    }

}
```



