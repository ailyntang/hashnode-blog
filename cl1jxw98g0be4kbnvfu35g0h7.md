## Delegates vs closures

They ultimately do the same thing.
But, sometimes one can be better than the other.

E.g. I had a factory struct, responsible for building UI elements. 

One of the UI elements was a `UISwitch`, and when the switch toggled, I wanted to update the view containing that switch.

There are two ways for that.
One is a callback, so something like:

```
// can be a struct or a class, could even be an enum I think

static func updateSwitch(callback: @escaping () -> Void) {
   // blah blah
   callback()
}
```

Another is a delegate:

```
protocol MySwitchProtocol: AnyObject {
  func updateView()
}

// must be a class, because it needs a `weak var`
final class MyFactory {
  weak var delegate: MySwitchProtocol?

  func updateSwitch() {
    // blah blah
    delegate?.updateView
  }
}
```

Then the view must set the delegate
```
final class MyView: UIViewController, MySwitchProtocol {
  let myFactory = MyFactory()
  myFactory.delegate = self

  func updateView() {
      // do something here
  }
}
```

As you can see, the delegate requires much more structure around it.
Sometimes maybe this is worth it.

But other times.. maybe not.

