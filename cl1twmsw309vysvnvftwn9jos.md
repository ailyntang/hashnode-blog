## ATT prompt not showing reliably

From iOS 14.5, apps need to show the ATT prompt when the user first loads the app.

However, that is the prime time to ask for all other prompts as well, like push notifications, location settings etc.

Since iOS 15, the other prompts can put the app in to an `inactive` state.
* In this inactive state, the ATT prompt does not show up.
* No ATT prompt means your app does not comply with Apple's rules, and will not get approved for sale

To fix this, if you are expecting other prompts:
* Check that `UIApplication.shared.applicationState == .active` before showing the ATT prompt
* Else if it's not active, add an observer that will show the ATT prompt as soon as the state changes to active

As always with observers, remember to remove the observer when the view is gone.


```
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        if UIApplication.shared.applicationState == .active {
            showATTPrompt()
        } else {
            NotificationCenter.default.addObserver(self,
                                                   selector: #selector(showATTPrompt),
                                                   name: UIApplication.didBecomeActiveNotification, object: nil)
        }


    @objc func showATTPrompt() {
        if #available(iOS 14.5, *) {
            ATTrackingManager.requestTrackingAuthorization { authorizationStatus in
                  // Do something here if you want to use the call back
            }
        }
    }


    override func viewWillDisappear(_: Bool) {
        super.viewWillDisappear(true)
        NotificationCenter.default.removeObserver(self,
                                                  name: UIApplication.didBecomeActiveNotification,
                                                  object: nil)
    }
```