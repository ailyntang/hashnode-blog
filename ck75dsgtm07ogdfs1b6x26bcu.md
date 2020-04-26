## Carthage: update only one binary

# Existing framework that isn't in the Cartfile

This means you added the framework a different way. Maybe through a podfile.

If you want to add it with Carthage, the first thing is to create a new branch in git, go into Xcode and delete the existing framework. Don't worry, you'll be adding it back soon enough.

I've forgotten if you delete it from the Build Settings, or just in the folder hierarchy. I think it's the folder hierarchy.

Find it under something like `Target > Frameworks folder`

![Screen Shot 2020-04-27 at 6.58.54 am.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1587934746282/Pdst2tg53.png)

# Add the new framework to your Cartfile

Using Carthage to manage your frameworks means that you will have a Cartfile.

This is where you tell Carthage which frameworks you want. It's like a Podfile for Cocoapods.

Your Cartfile will look something like:
```
github "Alamofire"
binary "https:www.rawgithubusercontent.com/blah/blah/name-of-framework.framework.json"
```

# Update the one framework you added

If you want to update only one dependency, then you use the commands below. Be careful - caps matter! You must use the correct capitilisation or carthage won't be predictable in what it does.

* If the framework starts with `github` in the Cartfile: `carthage update Alamofire`
* If the framework starts with ` binary`: `carthage update name-of-framework`. 

Note if you get the error "No entry found for dependency xyz in Cartfile", go and check that you have properly used `github` or `binary`. You can't mix and match these.

Frameworks that end in `.json` should be `binary`.

# Add the framework to Xcode

* Open Finder

* Navigate to your project folder > Carthage > Build > iOS. In here you should see the new framework that you just updated.

* Drag the entire folder over into Xcode. The place in Xcode that you want to drag and drop to is in the key target. So head up to your project, then Look for "Targets". Go to the "General" setting, then scroll down til you see "Frameworks, Libraries, and Embedded Content". Drag and drop the folder in here. It will add a little briefcase looking item to the list
