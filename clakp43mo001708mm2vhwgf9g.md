# Visual debugger: ambiguous height and vertical position

If you get this warning with a stackview, and it is fine with one element in the stackview but appears with multiple elements in the stackview, check the following:

* Ensure you have set `stackview.distribution`. So you may need `.fillProportionally`
* This can happen if you have stack views within stackviews. Let's say you have a title and and a subtitle `UILabel` in a vertical stackview. And so you'll need to have set `setCompressionResistancePriority` and `setCompressionHuggingPriority` on the labels, so that the vertical stackview knows how to grow and wrap the text labels
  * Then you start putting those child stack views into a parent stack view. Each child could be of different height, depending on the length of the text in the labels. So if the parent stackview has the distribution set to `fill`, it will get confused, as the children are differing heights. 
  * The fix: set the parent stackview distribution to `fillProportionally`, and then the parent knows each child can be a different height