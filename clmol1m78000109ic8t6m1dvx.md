---
title: "Using `UIView` inside `SwiftUI: View`"
datePublished: Mon Sep 18 2023 07:46:32 GMT+0000 (Coordinated Universal Time)
cuid: clmol1m78000109ic8t6m1dvx
slug: using-uiview-inside-swiftui-view
tags: swiftui

---

First: create a new view that adheres to `UIViewRepresentable`

* e.g. `struct SwiftUIVersion: UIViewRepresentable`
    
* This is the `SwiftUI` version of your `UIView`.
    

---

Then, inside your original UIView, you need to add:

```plaintext
    override var intrinsicContentSize: CGSize {
        let height = containerView.systemLayoutSizeFitting(UIView.layoutFittingCompressedSize).height
        return CGSize(width: UIScreen.main.bounds.width - .gutter.doubled, height: height)
    }
```

* This allows SwiftUI to figure out the size of your view
    
* Without this code, your view will use up too much vertical padding, and it's horizontal width will be too short or long, depending on the text inside the `UILabel`
    

---

Next, in your `SwiftUI` code, add your new view and append `.fixedSize()` to reference the `intrinsicContentSize` you just added.

---

Now - notice that your labels truncate if they are too long.

* This is despite the fact they are wrapping correctly with `UIKit`
    
* You have to handle the labels again, using `labelToWrap.setContentCompressionResistancePriority(.defaultLow, for: .horizontal)`
    
* Why low? blergh. I don't know. It's late. My brain is fried, but this seems counter to what I would expect.
    

---

Now - notice that if your label is too short, vs too long, the spacing is different.

This is frustrating. I don't know why this is.

---

Some SO articles that helped me:

[https://stackoverflow.com/questions/52736507/type-uiview-has-no-member-layoutfittingcompressedsize](https://stackoverflow.com/questions/52736507/type-uiview-has-no-member-layoutfittingcompressedsize)

[https://stackoverflow.com/questions/67147350/uiviewcontrollerrepresentable-height-is-too-big](https://stackoverflow.com/questions/67147350/uiviewcontrollerrepresentable-height-is-too-big)

[https://stackoverflow.com/questions/67786835/multiline-label-with-uiviewrepresentable](https://stackoverflow.com/questions/67786835/multiline-label-with-uiviewrepresentable)