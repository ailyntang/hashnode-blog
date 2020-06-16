## General swift and Future concepts

I'm currently using a home grown `Future` library. The setup combines three concepts that I ~rarely~ never write on my own:

* Generics
* Functions as parameters
* Error throwing
* Closures

Here's an example of a function declaration that combines all three:

```
func then<V>(coolFunction: @escaping (T) throws -> V) -> Future<V>
```

## Generics 

There are two generics in here: `V` and `T`.
Now we understand them, let's re-write it without the generics so they aren't so confusing.

We'll pretend `V` is always of type `Bool`, and `T` is always of type `Int`.

Note that because these are generics, either of these types could be `Void` in reality


```
func then(coolFunction: @escaping Int throws -> Bool) -> Future<Bool>
```

## @escaping

This is used for closures typically.
It means that whatever you return here can be used elsewhere in your code base. i.e. it can "escape".

Here's an example function.

```
func isItTimeForChocolate(time: String) -> Bool {
   let chocolateTime: String = "now"
   return time == chocolateTime
}
```

This function has an internal variable called `chocolateTime`. No one outside of this function knows that `chocolateTime` exists. E.g.

```
func isItTimeForChocolate(time: String) -> Bool {
   let chocolateTime: String = "now"
   return time == chocolateTime
}

print(chocolateTime) // This will fail to compile: chocolateTime doesn't exist outside the function
```

This is a very simplified concept of something that is not escaping. 
In this simple example, if we did want to use `chocolateTime` outside of the function, we would declare it outside the function, as it's just a `String`.

However in closure life and our `then` function, things are a little more tricky.
So to make sure we can use elements outside of the function, we use `@escaping`.

The `@escaping` keyword tells us that we can still access and see `Int` outside of the `coolFunction`.
```
func then(coolFunction: @escaping Int throws -> Bool) -> Future<Bool>
```

So now we know what the purpose of `@escaping` is, let's remove it from our function to make it a bit simpler to understand.

```
func then(coolFunction: Int throws -> Bool) -> Future<Bool>
```

## throws

`throws` denotes error handling.
The  [Swift documentation](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html) does a nice job of explaining it.

Let's look at a simple function:

```
func isItCakeTime() -> Bool {
    return true
}
```

This function always returns a `Bool`. However what happens if there is no cake, and you want to return an error?
Usually I would make it return an optional, i.e. `Bool?`.

```
func isItCakeTime() -> Bool? {
    guard isThereCakeAtHome else { return nil }
    return true
}
```

However this assumes that the user of the function knows that `nil` is actually an error state. Often `nil` is not an error, it is an acceptable output.

If I wanted to explicitly call out that this is an error, not an acceptable output, then I would use `throws`.

```
enum CakeError {
    case noMoreCakeLeft
    case notARelaxingTimeToEatCake
}

func isItCakeTime() throws -> Bool {
    guard isThereCakeAtHome else { throw CakeError.noMoreCakeLeft }
    guard isBabyAsleep else { throw CakeError.notARelaxingTimeToEatCake }
    return true
}
```

So now we can look at our original function and understand that `throws` denotes the ability of that function to throw an error.

Nothing more complex than that.

```
func then(coolFunction: Int throws -> Bool) -> Future<Bool>
```

If we didn't want it to throw an error, we could simplify it to:
```
func then(coolFunction: Int -> Bool) -> Future<Bool>
```

## Functions as parameters

The `then` function takes only one parameter, `coolFunction`.

Usually I'm used to parameters having simple types, like `String`.
However in this case, we want the parameter to be a function instead of a `String`.
Note that these two statements are the same:
```
func then(coolFunction: Int -> Bool) -> Future<Bool>
func then(coolFunction: (Int -> Bool)) -> Future<Bool> // this is the same as above, but with () around (Int -> Bool)
```

When I add the parentheses around `(Int -> Bool)`, I find it a lot easier to understand that `coolFunction` is actually expecting another function.

It's not just expecting any function though. It's expecting a function which takes an `Int` and returns a `Bool`. 
(Actually, we want a function that takes a generic parameter, `T`, and returns another generic parameter, `V`).

So now we understand this bit, we could simplify the function to:
```
func then(coolFunction: String) -> Future<Bool>
```

Ta-dah! Now we have a simple function declaration that we understand!

Looking back at the original function declaration, hopefully it's a bit less tricky now:
```
func then<V>(coolFunction: @escaping (T) throws -> V) -> Future<V>
```

## Using the function

Here's a simple example of how to use `then`.

```
private var secretCakeRecipe: String = ""

then { [weak self] recipe in
   self?.secretCakeRecipe = recipe
   return true
}
```

In this example, everything inside the `{ }` is the `coolFunction`. So the `coolFunction` takes an input, which is `recipe` and it returns a `Bool`.

`then` doesn't know what type `recipe` is. In this instance it is a `String`. But it could be an `Int` or anything else.

`[weak self]` exists because we don't want a retain cycle, as we are referencing a variable, `secretCakeRecipe` inside this closure.

It is also possible to remove `recipe` and just use $0:
```
private var secretCakeRecipe: String = ""

then { [weak self] in
   self?.secretCakeRecipe = $0
   return true
}
```

However if for some reason, we weren't using the input value `T` at all inside the function, then we would need to use `_` to tell the closure that yes, there is an input value but no, we won't be using it.

```
then {  _ in
   return true
}
```


## Final note

You may have realised I've ignored the final `-> Future<V>`

```
func then<V>(coolFunction: @escaping (T) throws -> V) -> Future<V>
```

It's a bit tricky to deal with this in isolation without the other future functions that exist around it.

So for now, I'll ignore it, sorry!















