## View lifecycles

`loadView()` is called once, when your view starts loading
* Initialise and add views


`viewDidLoad()` is called once, when your view finishes loading
* Not sure what you do here

`viewWillAppear()` is called every time you come back to your view
* API calls to refresh data?

`viewDidAppear()` is called every time you come back to your view, called after `viewWillAppear()`
* We put analytics tracking for page views here. We used to put them in `viewWillAppear`, but this was not reliable for some reason. Even though theoretically, it should be ok. Logically, it makes more sense to me anyway that you'd only track an analytics page view after the view appeared, rather than in `viewWillAppear`
