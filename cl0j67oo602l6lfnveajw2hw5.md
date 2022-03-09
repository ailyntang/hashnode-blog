## Xcode project / target / build settings

# Targets

* A project can have multiple targets.
* It must always have at least 1 target, which will be your iOS app, or watchOS app etc
* If you are going to have unit tests, this must be a separate target. 
  * It's required by Apple. 
  * But if it wasn't required by Apple, you could theoretically stuff it into your iOS app target.
    * This would be great coding though, because that means you are forcing the consumer of your app to include non-essential app code. So it would bloat the size of your app


# Build config

* Applies to the entire project
* Customises what you do. So you could have a config for "Marketing team" which has the app name "Marketing yo yo". 
* Typically you'll have a config for "production", "development" and maybe "QA" and a few others

