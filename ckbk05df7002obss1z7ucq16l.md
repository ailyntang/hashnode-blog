## Retain cycles

## Closures are non-escaping by default
* This means there is no need to put in `[weak self]` even when using `self`. As soon as the closure is finished, it will clean up after itself and there is no possibility that something else will have an unexpected reference as the closure is non-escaping.
* `.forEach` is an example of a closure, and it is non-escaping
* You can always put in `[weak self]` to be safe, but it's not required. And probably good practice to really think about why you are putting it there, rather than just scattering the whole code base with unnecessary `[weak self]`
*  [This article](https://www.avanderlee.com/swift/weak-self/) is super helpful. Basically - put `deinit` everywhere to debug and see if things are being retained unexpectedly. 
* If a closure references a function, I think in most instances, you don't need `[weak self]`. Even if the function then accesses variables.
*  [Another helpful article](https://www.swiftbysundell.com/questions/is-weak-self-always-required/) about the general concept of when you have a retain cycle vs not.

 [This article](https://matteomanferdini.com/swift-weak-self/) talks specifically about timers. I implemented what it said would cause a retain cycle, then tried to catch the retain cycle using `leaks` and also `deinit`. But I couldn't catch it out :( So now I'm not too sure how to catch the retain cycle.

## Only `@escaping` closures have potential issues with retain cycles
* The only time a retain cycle should be created is if the closure STORES a value
* If it's just accessing a function, but nothing is stored, then there shouldn't be a retain cycle
* You can always put in `[weak self]` to be safe, but not a big deal

For the animate case, where we have:
```
open class func animate(withDuration duration: TimeInterval, animations: @escaping () -> Void)

UIView.animate(withDuration: 0.3,
                       delay: .zero,
                       options: .curveEaseOut,
                       animations: {
                           self.view.layoutIfNeeded()
                       })
```
Typically developers won't both with `[weak self]` in this case, even though `animations` is an `@escaping` function. 

This is because the closure is completed and disposed of after the animation. Typically the animation is so quick (0.3 seconds in this case), that there's very little chance of a retain cycle.

However if you have a longer animation, yes, this could be an issue.

So I think for safety, it should be `animations: { [weak self] in self?.view.layoutIfNeeded() }`


## Check if you have a memory leak

 [This article](https://rderik.com/blog/using-xcode-s-visual-debugger-and-instruments-modules-to-prevent-memory-overuse/) is great. Tells you how to use `leaks` in the command line to see if you have a memory leak.

## `guard let self = self else { return }`
* This makes a strong reference to `self` inside the rest of the closure
* Use this to make life easier on yourself when coding, so you don't have to handle the `nil`
* However note if you have nested closures after the `guard` statement, then these nested closures will hold a strong reference to `self`. So you will need to re-declare `[weak self]` again in those nested closures.
* It's not a requirement, but ideally if you are going to declare `weak self` with nested closures, you may as well declare it on the overarching closure, rather than just the closure you think needs it. Then everything is safe.

## `if let self = self`
* This only makes a strong reference to `self` inside the `if` statement
* Outside the `if` statement, other `self` references will be weak

Autoclosure - take a look at this concept