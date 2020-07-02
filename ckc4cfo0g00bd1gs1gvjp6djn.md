## When titleLabel.text = "Hi" crashes your app

This happens when you are using storyboards and xibs.

Here are some reasons your app could crash:
* You forgot to connect the outlet to your `.swift` file
* You are calling `titleLabel.text = "xyz"` too early. It needs to be done in `viewDidLoad`, or sometime after the view is presented. It can't be called before the view is presented.
* You are instantiating a class instead of a nib (or vice versa)