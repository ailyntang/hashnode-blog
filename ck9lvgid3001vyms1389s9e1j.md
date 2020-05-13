## Closures and [weak self]

In closures, if you call a reference to `self`, you should use `[weak self]`. This ensures the memory is deallocated, as only a weak reference to that object is kept. 

If you don't use `[weak self]`, a strong reference is the default, and when that closure finishes running, the memory will hold onto that reference but there will never be a way for it to clean itself up.

Example:
```
button.tap { [weak self] in
    self?.showNextScreen()
}
```

However sometimes you will see people include a `guard` statement as well:
```
button.tap { [weak self] in
    guard let self = self else { return nil }
    self.showNextScreen()
    self.logAnalyticsEvent()
}
```

This `guard` makes the reference to `self` strong inside the closure (I think that's the right way of phrasing this).

It's handy for two reasons (taken from my great coworker):
* the closure has multiple references to self; in this case, theoretically, the object referenced could be deallocated between uses of the reference resulting in an unexpected state.
* the closure requires a non-optional reference of self to be passed into some function

The other big reason for making `self` strong, is if you want all the elements to continue as a block.

For example, if `self?.showNextScreen()` fails, then you probably don't want `self?.logAnalyticsEvent()` to run. So making `self` strong means that either both will fail or both will go through.
