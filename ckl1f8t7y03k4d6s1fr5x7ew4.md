## Add default values to protocol functions

## One default value
```
protocol Tasty {
   func makeCake(flavour: String, layers: Int)
}
```

Ideally want to have a default value for `layers: Int = 1`. However you can't do that in a protocol.

You need to make a protocol extension.

```
extension Tasty {
   func makeCake(flavour: String) {
       makeCake(flavour: flavour, layers: 1)
   }
}
```

Note what is happening:
* The extension function does not have the parameters with default values. ie. `layers` is missing as a parameter. This is essential to provide the default behaviour.
* The default value is specified inside the function, ie. `layers: 1`


## Two or more default values

Be careful. You can have default values in both the protocol extension, AND in the class implementation of the function.

However I think that's a bit confusing, and may lead to users unexpectedly receiving a durian cake instead of chocolate. Which would be devastating.

```
import Foundation

protocol Tasty {
    func makeCake(flavour: String?, layers: Int, cream: Bool)
}

extension Tasty {
    func makeCake(cream: Bool) {
        makeCake(flavour: "chocolate", layers: 1, cream: cream)
    }
}

class Lunch: Tasty {
    func makeCake(flavour: String? = "durian", layers: Int, cream: Bool) {
        print("\ntasty cake made!")
        print(flavour)
        print(layers)
        print(cream)
    }
}

let todaysLunch = Lunch()
todaysLunch.makeCake(flavour: "strawberry", layers: 3, cream: true)
todaysLunch.makeCake(layers: 11, cream: false) // cake flavour is durian
todaysLunch.makeCake(cream: true) // cake flavour is chocolate
```

The better way, in my opinion, is to put all the default values in the protocol extension only.

But this means you'll need to provide all the different possible parameter combinations in the extension. 

If you have 2 default values, then you'll need 4 different function declarations. One will be in the protocol (with all parameters provided); three will be in the protocol extension (with various combinations of missing parameters).

Now it's really clear where all the default values lie. 

```
protocol Tasty {
    func makeCake(flavour: String?, layers: Int, cream: Bool)
}

extension Tasty {
    func makeCake(flavours: String?, cream: Bool) {
        makeCake(flavour: flavours, layers: 1, cream: cream)
    }
    
    func makeCake(layers: Int, cream: Bool) {
        makeCake(flavour: "chocolate", layers: layers, cream: cream)
    }
    
    func makeCake(cream: Bool) {
        makeCake(flavour: "chocolate", layers: 1, cream: cream)
    }
}

class Lunch: Tasty {
    func makeCake(flavour: String?, layers: Int, cream: Bool) {
        print("\ntasty cake made!")
        print(flavour)
        print(layers)
        print(cream)
    }
}

let todaysLunch = Lunch()
todaysLunch.makeCake(flavour: "strawberry", layers: 3, cream: true)
todaysLunch.makeCake(layers: 11, cream: false) // chocolate. 
todaysLunch.makeCake(cream: true)
```






