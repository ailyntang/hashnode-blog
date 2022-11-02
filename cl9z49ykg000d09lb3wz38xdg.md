# UIScrollView tips

```
let scrollView = UIScrollView()
let contentView = UIView()
let myTextField = UITextView()

scrollView.translatesAutoresizingMaskForAutolayoutConstraints = false
contentView.translatesAutoresizingMaskForAutolayoutConstraints = false
myTextField.translatesAutoresizingMaskForAutolayoutConstraints = false

scrollView.addSubview(contentView)
contentView.addSubview(myTextField)

NSLayoutConstraints.activate([
   contentView.topAnchor.constraint(equalTo: scrollView.topAnchor),
   contentView.bottomAnchor.constraint(equalTo: scrollView.bottomAnchor),
   contentView.leadingAnchor.constraint(equalTo: scrollView.leadingAnchor),
   contentView.trailingAnchor.constraint(equalTo: scrollView.trailingAnchor),
])
```

I added a bunch of text fields to a scroll view, and for some reason, I couldn't tap on the text field.

In the visual ui debugger, Xcode gave me the warning: `scrollable content size is ambiguous`

However, the underlying issue was actually due to the content view inside the scroll view not having a fixed width and height.

Once I fixed that, everything worked :)

So - the fix was adding these two constraints:
```
NSLayoutConstraints.activate([
   contentView.heightAnchor.constraint(equalTo: view.frameHeight), // note, you need to play around with this constant to get it right for your scroll content
   contentView.widthAnchor.constraint(equalTo: view.frameWidth),
])

```


Red herrings that were totally unrelated, and didn't fix the bug:
* setting `scrollView.isUserInteractionEnabled = true`: this wasn't relevant, they already defaulted to `true`
* setting a value for `scrollView.contentSize`