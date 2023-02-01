# Cannot convert return expression of type 
'Blah<T>' to return type 'Blah<T>'

Ahhhh ok. So, it's weird.

The two types are the same!

Why is the compiler angry?

If you ever see this, it's because there are two DIFFERENT generics. So the compiler doesn't know if `T` really `==` `T`.

```plaintext
final class SomeClass<T> {

    func someFunction<T>() -> Blah<T> {
         return Blah(()) // COMPILE ERROR HERE
    }
```

So the issue is that we have `final class SomeClass<T> {`

and we ALSO have `someFunction<T>()`

So we are saying - the class takes a generic, `T`

Then the function takes ANOTHER generic, ALSO called `T`

And that's why the compiler is confused.

So, if you REALLY mean to have two different generics, then RENAME one of them from `T` to a different letter.

Otherwise, if the class AND the function are meant to reference the SAME generic, then delete the generic from the function declaration. So just have `func someFunction() -> Blah<T>` (instead of `someFunction<T>()`)

And this will compile now.