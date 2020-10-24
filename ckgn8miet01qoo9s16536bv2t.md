## Troubleshooting: UIButton title is always `nil`

Sometimes `UIButton.currentTitle` is always `nil`, even though you have set it using interface builder and using `UIButton.setTitle()`.

In this instance, use `UIButton.titleLabel?.text`. Theoretically when I read the documentation, these two are meant to point to the same thing. `.currentTitle` is read only, and `.titleLabel` is read and write.

However `.titleLabel` always returns what I expect, whereas `.currentTitle` sometimes is unexpectedly `nil`, even though I have set the text programmatically or through IB.

So for me, I'm going to always use `.titleLabel?.text` going forward.