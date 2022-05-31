## SDKs / frameworks which don't compile

Apple now only supports Swift Package Manager.
This is incredibly easy to use, if your SDK / framework is using SPM.

However.. many smaller frameworks aren't yet using SPM.
Some use `xcframework` (which Apple still kinda supports). 

Others that are even older still use `framework`. And sadly, Apple just straight up does not support this.

The problem with `framework` is that it doesn't work with the new M1 arm64 architecture. 

*Take a read of [this prior article](https://coderlyn.hashnode.dev/understanding-binaries-when-archiving-a-build-wip-ckt7zk7x80mi195s15tn51ljn) which explains more about builds, binaries and architectures.
*



So, what to do when the framework doesn't support the new M1?
Or, even worse, some frameworks don't even support the iOS simulator, such as the [Ada framework](https://github.com/AdaSupport/ios-sdk/releases) as at version 1.4.2.

Let's tackle them one at a time.

# Framework doesn't support the correct build, such as iOS simulator

* Download the source file, i.e the `Source.zip`
* Unzip the file
* In terminal, navigate to where you have your file. ie. `cd <path>`
* `mkdir temp` // all the new files will go into this folder
* Run these scripts (copy and paste the entire block into terminal, then press enter)
  * Build one archive for simulator (x86 and arm64-ios-simulator) 

```
xcodebuild archive \
-scheme AdaEmbedFramework \
-destination "generic/platform=iOS Simulator" \
-archivePath temp/ada-sim.xcarchive \
SKIP_INSTALL=NO \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES
```

  * Build one archive for device (arm 64)

```
xcodebuild archive \
-scheme AdaEmbedFramework \
-destination "generic/platform=iOS" \
-archivePath temp/ada-device.xcarchive \
SKIP_INSTALL=NO \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES
```


  * Glue them together into an xcframework

```
xcodebuild -create-xcframework \
-framework temp/ada-sim.xcarchive/Products/Library/Frameworks/AdaEmbedFramework.framework \
-framework temp/ada-device.xcarchive/Products/Library/Frameworks/AdaEmbedFramework.framework \
-output temp/AdaEmbedFramework.xcframework
```

* Now you can add the framework to your Xcode project. For Ada, you still need to embed and sign.


# Using carthage binaries to support M1

Another way is to use carthage binaries if you don't have access to the source code.
I'll update this another time. It's not as nice as just making your own xcframework.