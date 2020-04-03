## Async patterns

Asynchronous calls: I normally only think about these in the context of API calls. There are other use cases for async calls I'm sure, but I don't know about them yet.

There are a few common patterns out there to deal with async calls.

## Callbacks

Probably the oldest pattern


## Result enum

It puts some syntactical sugar on the call backs. The enum returns a failure or success.

This helps you to avoid having lots of nested statements inside the callback.

## Promises

Similar to the concept of `Future`s. 

Race conditions are common pitfalls in promises.

* Things return in different orders as they are all async. So if you rely on the order to be the same, you will get errors.
* `debounce` waits until you get everything back then returns only the last request. People use `debounce` to try and avoid race conditions. But you don't know what the last request will be as they are all completing asynchronously.

# Reactive patterns

The next generation of async patterns is reactive patterns.

These are known as observable/observer or publisher/subscriber. One set is the naming convention from Apple, and the other set is the naming convention outside of Apple.

The observer only runs if someone subscribes to it. Otherwise that code will never be seen. As soon as there is a subscriber, the observer runs its code.

# Handling async in Swift

There are two existing frameworks: RxSwift and Combine. RxSwift is easier to use than Combine.



