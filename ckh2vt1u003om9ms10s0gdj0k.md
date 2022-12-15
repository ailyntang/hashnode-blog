# Corner radius doesn't work

Most of the time `view.layer.cornerRadius = 10` works fine.

However if you notice that it's not working, you are likely missing: `view.clipToBounds = true`. This property is set to false by default.

One instance of where you need to clip to bounds: when you have your custom view pop up as a modal inside a view controller.

* * *

Ok.. another reason it may not work - custom UI views.

Sooo.. I had applied the corner radius to my custom view, `ProgressBarSection`

I then had a parent view, `TableViewCell` with a property `let section: ProgressBarSection` 

In order for the section to properly have the corner radius set, in the parent table view cell, I also needed to write `section.layer.cornerRadius = xx`