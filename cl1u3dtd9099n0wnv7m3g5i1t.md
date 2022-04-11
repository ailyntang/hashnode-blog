## Debugging tips: cross referencing from Crashlytics to Amplitude

# Find how many users experienced an issue on an API route
* Your app needs to send the api route as a custom key to firebase
* Crashlytics > Event > Tap into an event that you want to deep dive into
* Crashlytics > Top filter > Custom keys > apiRoute

Note it's a bit buggy. You may need to click in and out a few times before you'll be able to deselect and select the actual apiRoutes.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649643808293/k3iNoJUFp.png)


# Take a user from Crashlytics and find them in Amplitude

If your app has sent the trace parent to Crashlytics, it will look like this:
`xx-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx-aaaaaaaaaaaaaaa-x`

Copy over the third group of symbols and numbers, the `aaaaaaaaaaaaaaa` string


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649643485674/KHyooHNXH.png)


In Amplitude, search for `Any active event` and filter for `aaaaaaaaaaaaaaa`


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649643616135/3AEkEf5O8.png)

Then you'll be able to pull up the user stream and see all the analytics events in Amplitude related to this user who experienced the Crash in Crashlytics


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649643672033/FZDdMYGE8.png)
