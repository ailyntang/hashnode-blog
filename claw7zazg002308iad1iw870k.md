# Cannot declare conformance to 'NSObjectProtocol' in Swift; `xyz` should inherit 'NSObject' instead

So.. this build error sometimes pops up when I'm trying to get a class to conform to a `UIKit` delegate, such as `UITextViewDelegate`.

```
final class MyCustomViewController: UIViewController {
  // lots of code
}

extension MyCustomViewController: UITextViewDelegate {
  // error appears
}
```

What this means: the custom class needs to conform to `NSObject` first, before it can conform to `UITextViewDelegate`

Note, with `NSObject`, you have to conform to it at the class declaration. Not in an extension.

So the fix to this error is this line: `final class MyCustomViewController: NSObject, UIViewController`