## Carthage: update only one binary

Using Carthage to manage your frameworks means that you will have a Cartfile.

This is where you tell Carthage which frameworks you want. It's like a Podfile for Cocoapods.

Your Cartfile will look something like:
```
github "Alamofire"
binary "https:www.rawgithubusercontent.com/blah/blah/name-of-framework.framework.json"
```

If you want to update only one dependency, then you use the commands below. Be careful - caps matter! You must use the correct capitilisation or carthage won't be predictable in what it does.

* If the framework starts with `github` in the Cartfile: `carthage update Alamofire`
* If the framework starts with ` binary`: `carthage update name-of-framework`. 

Note if you get the error "No entry found for dependency xyz in Cartfile", go and check that you have properly used `github` or `binary`. You can't mix and match these.

Frameworks that end in `.json` should be `binary`.