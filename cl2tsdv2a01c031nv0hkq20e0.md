## OptionSet and bitwise operator <<

I don't really get `OptionSet` or bitwise properly.
But this is a starting point before starting to google.

----

`OptionSet` is a slightly different use case to `enum`

Imagine you are at a Pizza Hut / Sizzler dessert buffet (if you are a child of the 80s). 
Otherwise you can imagine you are at a frozen yoghurt place. Much more modern, but no nostalgia there.

You have your soft serve, and you have infinite toppings you can choose from.

This store charges by number of toppings the customer picks, not weight.

So, when you implement it, you could have:

```
enum Topping {
    case sprinkles
    case honeycomb
    case strawberries
    case cookieDough
    case peanuts
}
```

Then when comes time to calculate the amount.. you'll need a `numberOfToppngs: [Topping?]`. Or something like that. Cos the customer may have 0 toppings, or all 5 toppings.

However, if you use an `OptionSet`, it handles this for you.

I think a better example for `OptionSet` may be a sushi train.
Cos the point of option set is that it could be many things.
* `10` = you have udon but no sashimi.
* `11`= you have udon AND sashimi


```
struct SushiOptions: OptionSet {
    static let udon    = SushiOptions(rawValue: 1 << 0)
    static let sashimi  = SushiOptions(rawValue: 1 << 1)
}
```

I think `OptionSet` always uses bitwise operations, [described here](https://docs.swift.org/swift-book/LanguageGuide/AdvancedOperators.html) by Apple.



-----

When to use bitwise operators instead of `Int`?
When you care about speed, e.g. when you are working with video or audio, speed matters.