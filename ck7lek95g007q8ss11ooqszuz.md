## Troubleshooting UIScrollView

# Basics

In AutoLayout, the scroll view will determine its content size automatically. 
If the size is smaller than the size of the scroll view, then there's no need for any scrolling action.

# View doesn't scroll

If the size is not smaller than the scroll view, but it's still not scrolling, then you probably haven't set the `contentInset`. Similar issue to the next topic.

The scroll view doesn't think it needs to scroll, because it doesn't realise that you want the display area to be smaller than it currently is. It's probably at its full height and displaying all the information it needs to, but the bottom of the view is hidden behind an element at the bottom of the screen.

Set `scrollView.contentInset.bottom = 100`, where `100` is the height of the element that is hiding the bottom of the scroll view.

# View still doesn't scroll

If you are using Frames instead of AutoLayout, then you will need to set the property `contentSize`. Otherwise the scrollview won't scroll.

You can set this in `viewDidLayoutSubviews()`. Online I have read that you should set this in `viewWillAppear`, because if you set it in `viewDidLoad`, there will be issues as `viewDidLoad` is called too early on.


# View bounces back to the top

One thing it could mean, is that the content inset needs to be set.

This happens if you have something that is fixed to the bottom of your view, like a floating action button with a white background. 

It's likely your scroll view is displaying stuff BEHIND that white background. So it's scrolling correctly, if the button background wasn't there.

You need to tell the scrollview to offset its bottom edge inset by the height of that button background. Then the scrollview knows it's stopping before the button begins.

To do that, use the following code, where `40` represents the height of the white background:

`myScrollView.contentInset.bottom = 40`


# Content size is ambiguous

Don't be fooled by the warning. You don't actually need to set the `contentSize` to fix this warning.

Chances are you have a scroll view that contains multiple elements, and you are adding each of these elements directly to the scroll view.

Instead, you should do the following:
1. Add a content view
2. Add the UI elements to the content view, not the scroll view
3. Constrain the content view to the scroll view
4. Explicitly set the height and width of the content view - these can equal `view.frameHeight` and `view.frameWidth`

In code, step 1 and 2 would look like:

```swift
// Inside viewDidLoad

view.addSubview(scrollView)
scrollView.addSubview(contentView)
contentView.addSubview(titleLabel)
contentView.addSubview(subtitleLabel)
contentView.addSubview(imageView)
contentView.addSubview(descriptionLabel)
// etc.

// Add constraints

// Don't forget to add `contentInset` if you need to
// You shouldn't need to add `contentSize`
```

# Articles
https://developer.apple.com/library/archive/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1