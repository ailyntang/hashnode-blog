## Tableview & collection view cells: Remove all tap gestures when preparing for reuse

When you use cells in a table view, they get reused with new content.

If you have tappable / interactive elements in those cells, such as buttons or carousels etc, you need to properly remove all the gestures as part of the reuse, otherwise odd things may happen.

This means you'll need to do something like:

```
func removeGestures(type: GestureType) {
        guard let gestureRecognizers = gestureRecognizers else { return }
        for gesture in gestureRecognizers {
            // clean out any gestures of matching type
            switch type {
            case .longPress:
                if let g = gesture as? UILongPressGestureRecognizer {
                    removeGestureRecognizer(g)
                }
            case .tap:
                if let g = gesture as? UITapGestureRecognizer {
                    removeGestureRecognizer(g)
                }
            case .swipeLeft:
                if let g = gesture as? UISwipeGestureRecognizer, g.direction == .left {
                    removeGestureRecognizer(g)
                }
            case .swipeRight:
                if let g = gesture as? UISwipeGestureRecognizer, g.direction == .right {
                    removeGestureRecognizer(g)
                }
            case .swipeUp:
                if let g = gesture as? UISwipeGestureRecognizer, g.direction == .up {
                    removeGestureRecognizer(g)
                }
            case .swipeDown:
                if let g = gesture as? UISwipeGestureRecognizer, g.direction == .down {
                    removeGestureRecognizer(g)
                }
            }
        }
    }
```

Then call on this remove function in the `prepareForReuse` function:

```
override func prepareForReuse() {
        super.prepareForReuse()
        button.removeGestures(type: .tap)
    }
```