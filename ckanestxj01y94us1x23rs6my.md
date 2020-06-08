## Layout subviews: don't add or remove views here

**Why not?**
LayoutSubviews can run multiple times while the view is presented or about to be presented.

Ideally don't add/remove views within layoutSubviews as can introduce duplicate view bugs.  

Especially if a view is regenerated based on another call. 

We could have multiple instances of a view hanging around as the object scoped reference gets replaced, but the existing view is kept alive as it's already in the view hierarchy.

Example of regeneration

```
var titleLabel = UILabel() // this generates the label in the properties

func someMethod() {
    if updateTitle {
        titleLabel = UILabel() // this is REgenerating the label in this method. Not a good idea.
    }
}
```

**What to do instead**

If the view is being regenerated, then add/remove the view in that call.

Even better would be you can add the view, update the view with the necessary information, and then hide it if it's not needed.

```
var titleLabel = UILabel() // this generates the label in the properties

func someMethod() {
    if updateTitle {
        titleLabel.text = "fancy new title" // note there is no = UILabel(), so this updates an existing label instead of regenerating it
    }
}
```

**Why would you ever regenerate?**

`UILabel` is not a great example, because it's to easy to update the label text using `titleLabel.text`, that you might wonder why any one uses regenerationg.

However imagine you have a custom object.

```
struct Icecream {
    init(flavour: String, toppings: String?) {}
}
```

Then you have a store that uses this object. You want to be able to deal with people changing their mind about the flavour.

You may have:
```
final class IceCreamStore {
    private lazy var icecream = Icecream(flavour: "chocolate")

    init() { }

    func changeFlavour(newFlavour: "peppermint") {
        icecream = Icecream(flavour: "peppermint") // there is no way to change the flavour of the icecream, so you have to regenerate it
    }
}
```

What would be better would be to update the struct so that the struct has a method that can update the ice cream flavour. Just like `UILabel` has a method that can update the text. 

Then you icecream store could call this:
```
func changeFlavour(newFlavour: "peppermint") {
        icecream.updateFlavour(to: "peppermint") // no regeneration of the initial struct, Icecream()
    }
```

