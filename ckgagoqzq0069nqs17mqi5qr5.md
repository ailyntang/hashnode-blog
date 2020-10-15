## `Timer` does not fire

Be careful - if you use 

```
Timer(withTimeInterval: 1.0, repeats: true, block: { _ in
            print("this will never execute unless you setup a run loop")
        })
```
then you will need to manually add the timer to a run loop.

Most likely you want to use `scheduledTimer` instead. Very similar:

```
Timer.scheduledTimer(withTimeInterval: 1.0, repeats: true, block: { _ in
            print("this will fire without the need to setup a run loop")
        })
```