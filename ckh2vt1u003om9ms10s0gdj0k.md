## Corner radius doesn't work

Most of the time `view.layer.cornerRadius = 10` works fine. 

However if you notice that it's not working, you are likely missing: `view.clipToBounds = true`. This property is set to false by default.

One instance of where you need to clip to bounds: when you have your custom view pop up as a modal inside a view controller. 
