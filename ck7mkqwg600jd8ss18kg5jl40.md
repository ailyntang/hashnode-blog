## Constraining stackviews

This SO article was helpful for me:
https://stackoverflow.com/questions/56138193/constraint-items-must-each-be-a-view-or-layout-guide

However I couldn't figure out the difference between which `attribute` to use and the `relatedBy`.

With normal constraints I got the result I wanted.
But I couldn't define the correct `NSLayoutConstraintAttribute`.

```
let topConstraint = NSLayoutConstraint(item: bottomLabel,
                                               attribute: .top,
                                               relatedBy: .equal,
                                               toItem: topLabel,
                                               attribute: .bottom,
                                               multiplier: 1,
                                               constant: 0)
myStackView.addConstraint(topConstraint)
```