## Updating constraints programmatically - WIP

Sometimes you need to update a constraint. E.g. you make an API call, and maybe you need the padding on the top anchor to update.

To do this, set up a variable: `topAnchor: NSLayoutConstraint`


Be careful - if the constraint is not optional, and then the constraint is removed.. your app could crash.

To synthesise these points later:

declaring the constraints as variables, and then initialising them somewhere else, and because Im using superview as one of the references, which could be nil. but the constraint accepts a optional UIView as the item is referencing.

Also, not relevant here, but might influence my thinking of declaring constraints as optional haha, when using storyboards and you reference a constraint in code, as with other IBoutlets, they are added as not optional, then if you remove them somewhere in the code they might not be valid anymore. and :boom: crash.