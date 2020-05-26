## Layout subviews: don't add or remove views here

**Why not?**
LayoutSubviews can run multiple times while the view is presented or about to be presented.

Ideally don't add/remove views within layoutSubviews as can introduce duplicate view bugs.  

Especially if a view is regenerated based on another call. 

We could have multiple instances of a view hanging around as the object scoped reference gets replaced, but the existing view is kept alive as it's already in the view hierarchy.

**What to do instead**

If the view is being regenerated, then add/remove the view in that call.

Or you can add the view, and then hide it if it's not needed.