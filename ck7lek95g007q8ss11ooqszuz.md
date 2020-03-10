## Scrollview bounces back to the top

One thing it could mean, is that the content inset needs to be set.

This happens if you have something that is fixed to the bottom of your view, like a floating action button with a white background. 

It's likely your scroll view is displaying stuff BEHIND that white background. So it's scrolling correctly, if the button background wasn't there.

You need to tell the scrollview to offset its bottom edge inset by the height of that button background. Then the scrollview knows it's stopping before the button begins.

To do that, use the following code, where `40` represents the height of the white background.
`myScrollView.contentInset.bottom = 40`