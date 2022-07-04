## Push notifications and deeplinks

**Deeplinks**

These can be triggered in three ways

- WITHIN the app. E.g. if you have a notifications feed
- OUTSIDE the app, via a push notification
- OUTSIDE the app, by tapping on link in Safari, e.g. `your-app://somedeeplink`

Deeplinks always have an associated `URL?` that Swift will need to handle, in order to open the deeplink and send the user to the correct place.

**Push notifications**

Push notifications are ALWAYS deeplinks.

Swift will first handle push notifications via the app delegate function and populating the `userInfo: [AnyHashable: Any]`:

```
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void)
```

This will open the app.

Once the app is open, then it is time to handle the deeplink's url, in order to send the user to the right place inside the app.
