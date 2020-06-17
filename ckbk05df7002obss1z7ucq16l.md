## Retain cycles

Closures are non-escaping by default
* This means there is no need to put in `[weak self]` even when using `self`. As soon as the closure is finished, it will clean up after itself and there is no possibility that something else will have an unexpected reference as the closure is non-escaping.
* `.forEach` is an example of a closure, and it is non-escaping
* You can always put in `[weak self]` to be safe, but it's not required. And probably good practice to really think about why you are putting it there, rather than just scattering the whole code base with unnecessary `[weak self]`

Only `@escaping` closures have potential issues with retain cycles
* The only time a retain cycle should be created is if the closure STORES a value
* If it's just accessing a function, but nothing is stored, then there shouldn't be a retain cycle
* You can always put in `[weak self]` to be safe, but not a big deal

`guard let self = self else { return }`
* This makes a strong reference to `self` inside the rest of the closure
* Use this to make life easier on yourself when coding, so you don't have to handle the `nil`
* However note if you have nested closures after the `guard` statement, then these nested closures will hold a strong reference to `self`. So you will need to re-declare `[weak self]` again in those nested closures.

`if let self = self`
* This only makes a strong reference to `self` inside the `if` statement
* Outside the `if` statement, other `self` references will be weak
