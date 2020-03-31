## Nested enums

```
enum Icecream {
   enum Flavour {
      case chocolate
      case vanilla
   }

   enum Toppings {
      case sprinkles
      case none
   }
   case flavour(Flavour)
   case toppings(Toppings)
   case none
}
```

The `enums` do not need to be defined inside `Icecream`, if you want to use them outside of `Icecream` as well.

E.g. maybe `Flavour` is shared across `enum Icecream` and `enum Cake`. So you would define it once globally, and not inside `enumIcecream`.

Then you should be able to access it like this:
```
let icecream: Icecream = .flavour.chocolate
```