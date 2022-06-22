## `UIView` - layout subviews when you edit layers

BUG: you have a `UIView` and it has something like rounded corners.
For some reason - the view isn't showing up correctly.
Either it's missing completely, or some of the corners aren't correctly rounded.

Why?

It's because when you round corners, you are touching a layer.

Whenever you touch a layer, you need to tell the view controller or the view to layout the subviews again.

Take a look at [this article](https://www.marcosantadev.com/calayer-auto-layout-swift/).

TL;DR: do the layer work inside the relevant layout function.
That's it.

No need to call `layoutIfNeeded()`.


```
// Inside a view controller

override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()
    // move the code that touches layers to here
}

// Inside a custom view

override func layoutSubviews() {
    super.layoutSubviews()
    // move the code that touches layers to here
}
```