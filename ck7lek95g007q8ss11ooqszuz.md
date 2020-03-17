## Troubleshooting UIScrollView

# Basics

In AutoLayout, the scroll view will determine its content size automatically. 
If the size is smaller than the size of the scroll view, then there's no need for any scrolling action.

# View bounces back to the top

One thing it could mean, is that the content inset needs to be set.

This happens if you have something that is fixed to the bottom of your view, like a floating action button with a white background. 

It's likely your scroll view is displaying stuff BEHIND that white background. So it's scrolling correctly, if the button background wasn't there.

You need to tell the scrollview to offset its bottom edge inset by the height of that button background. Then the scrollview knows it's stopping before the button begins.

To do that, use the following code, where `40` represents the height of the white background:

`myScrollView.contentInset.bottom = 40`

# View doesn't scroll

If you are using Frames instead of AutoLayout, then you will need to set the property `contentSize`. Otherwise the scrollview won't scroll.

You can set this in `viewDidLayoutSubviews()`. Online I have read that you should set this in `viewWillAppear`, because if you set it in `viewDidLoad`, there will be issues as `viewDidLoad` is called too early on.

# Articles
https://developer.apple.com/library/archive/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1