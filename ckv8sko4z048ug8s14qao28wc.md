## Firebase custom traces

Sometimes you want to trace things, like load times of certain sections in your app.

### Step 1: Put it in your code

Be careful: if you are using something like `trace[id]`, make sure you force this to be on the main queue, and a specific queue.
Dictionaries are not thread safe.

Use something like:
```
let queue = DispatchQueue(label: "myCustomTrace")
queue.sync {
    // code with `trace[id]` in here
}
```

### Step 2: Kill the app

After using the app to trigger the traces, you need to kill the app.
This ensures that the data is sent to Firebase, as these type of data are batched to send on app termination.

Sending data on demand is expensive for the battery and network. So Firebase batches its data to send at certain times.

App termination should guarantee it happens.

### Step 3: Find the trace in Crashlytics


Then to find your custom trace in Crashlytics:

Click on the performance tab:
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635295157580/Q_g4GEK_8.png)

Then top left - ensure you are viewing the right project, if you have development and production apps.


Also top left - then click on the big Performance tab and select which platform you want, Android or iOS. This is very subtly hidden!


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635295220867/0OjfV3R7A.png)