## Firebase custom traces

Sometimes you want to trace thing, like load times of certain sections in your app.

Be careful: if you are using something like `trace[id]`, make sure you force this to be on the main queue, and a specific queue.
Dictionaries are not thread safe.

Use something like:
```
let queue = DispatchQueue(label: "myCustomTrace")
queue.sync {
    // code with `trace[id]` in here
}
```


Then to find your custom trace in Crashlytics:

Click on the performance tab:
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635295157580/Q_g4GEK_8.png)

Then top left - ensure you are viewing the right project, if you have development and production apps.


Also top left - then click on the big Performance tab and select which platform you want, Android or iOS. This is very subtly hidden!


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635295220867/0OjfV3R7A.png)