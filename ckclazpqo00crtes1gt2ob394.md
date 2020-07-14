## Using Crashlytics

The stack trace gives you a snapshot in time.
But it doesn't always tell you WHY.
Sometimes you might be lucky, and the stack trace may give you enough.

But if it doesn't, and you can't reproduce it, then look at the LOG.

The log will tell you the path the user took to get to that point in time.

E.g. stack trace may say: they were on this view controller, pressed this button and then the app crashed.
When you go to that vc and press the button, nothing crashes for you.

But then you look at the log, and through that you can piece together that the user had backgrounded the app, come back through to that vc. And maybe that helps you repro the app.

Often the crashes are not straight forward to reproduce given our QA practices.

In order to fix the bug, you need to be able to reproduce it in the sim. So check the version number of the app, the software as that makes a difference. The device also makes a difference - older phones are slower, and there are race conditions that occur on older devices that won't occur on newer devices or on the sim.


For my company, we track non fatal errors as well. It's an alert that says, each time we notice an error, let us know. Even if it doesn't crash the app for the user. Often these errors, such as `NSURLError` means that the user has lost connectivity. This could be for many reasons. One reason is that our server is down. But most likely if it's just one user here and there, or if it happens all over the app, then it's probably not our company that's causing the issue. Perhaps the user just went into a tunnel, is in a patchy zone, or using a proxy catcher like Charles.

We care about it when there are many issues in the same location of the app, or if there are many issues full stop, because that is likely to be caused on our end.