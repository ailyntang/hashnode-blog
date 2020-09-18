## MVVM with storyboards: it's not possible

How interesting. I didn't realise this.
This [Medium article](https://medium.com/wolox/viewmodel-injection-in-viewcontrollers-with-storyboards-50230143c3ac) is great and explains how to work around it. The required "fix" is quite detailed.

Personally I don't think it's worth the effort to use storyboards in my situation, as the main storyboard is really basic.

Rather I'll use `.xib` files instead. This way I can programatically initialise the view controller and inject a view model simply, rather than needing the detailed fix outlined in the Medium article.

