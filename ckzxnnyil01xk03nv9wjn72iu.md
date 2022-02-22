## How to refresh `UIView`

Let's say you have a `UIView`, and you want to update what it looks like after fetching some data.

So you're after the equivalent of `tableView.reloadData()`, but for a `UIView`.

A search on Stack Overflow suggests that you can use `yourView.setNeedsLayout()`.
But that doesn't seem to work.

So then you also try `yourView.layoutIfNeeded()` as well.
Nope.

What is happening?

You have likely done this:
```
var someView: UIView()

view.addSubview(someView)

// later in some async data call
someView = makeNewView()
someView.setNeedsLayout() // etc.. doesn't work

```

You have already added the view.
So, now you need to do:

```
someView.removeFromSuperView()
someView = makeNewView()
view.addSubview(someView)
```

IMO it's not very nice.
Much better if you can just render elements of the view, instead of completely replacing the view.

But.. sometimes it can't be helped.