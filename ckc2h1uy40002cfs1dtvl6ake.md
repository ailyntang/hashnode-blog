## When overriding `defaultHeight`...

Call `layoutImmediately()` as part of the override. Otherwise there could be introduced timing bugs.

E.g. 
```
override var defaultHeight: CGFloat {
    layoutImmediately()
    return // whatever logic you want here
}
```