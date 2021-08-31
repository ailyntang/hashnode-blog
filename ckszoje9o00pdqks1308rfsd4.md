## Upgrading / Updating to new version of Xcode or frameworks

## Upgrading Xcode

**1. Test on device and on simulator**

* Run the project on device, and on simulator
* If it runs on both, there is nothing further we need to do to move over to Xcode
* If the project doesn't compile, then follow the steps outlined below

**2. Download the updated version of the offending framework**

* Find the github repo for the framework > Releases. This is the repo for the [Ada framework](https://github.com/AdaSupport/ios-sdk/releases).
* Download the framework if it exists
* If it doesn't, then download the source code and use the carthage script to build it

**3. Build framework using carthage script (if needed)**

* Create a new file, `carthage.sh` inside the same folder as the downloaded framework
* Copy and paste the below script into the new file
* Ensure Xcode is set to the new version: `sudo xcode-select --switch the/path/to/the/new/xcode/Contents/Developer/`
* In terminal, navigate to the folder with the new framework and run: `./carthage.sh build --no-skip-current --no-use-binaries`
* If you have see a permissions error when you try to execute the above command, try putting `bash` at the front
* The new framework will be built inside the `Carthage` folder of the framework

```
#!/usr/bin/env bash
# carthage.sh
# Usage example: ./carthage.sh build --platform iOS
set -euo pipefail
xcconfig=$(mktemp /tmp/static.xcconfig.XXXXXX)
trap 'rm -f "$xcconfig"' INT TERM HUP EXIT
# For Xcode 12 make sure EXCLUDED_ARCHS is set to arm architectures otherwise
# the build will fail on lipo due to duplicate architectures.
echo 'EXCLUDED_ARCHS__EFFECTIVE_PLATFORM_SUFFIX_simulator__NATIVE_ARCH_64_BIT_x86_64__XCODE_1200 = arm64 arm64e armv7 armv7s armv6 armv8' >> $xcconfig
echo 'EXCLUDED_ARCHS = $(inherited) $(EXCLUDED_ARCHS__EFFECTIVE_PLATFORM_SUFFIX_$(EFFECTIVE_PLATFORM_SUFFIX)__NATIVE_ARCH_64_BIT_$(NATIVE_ARCH_64_BIT)__XCODE_$(XCODE_VERSION_MAJOR))' >> $xcconfig
export XCODE_XCCONFIG_FILE="$xcconfig"
carthage "$@"

```

What does this script do?

* It replaces the duplicated binaries that were introduced with Apple Silicon
* Apple knows about this issue, and advises all frameworks to switch over to either Swift Package Manager or to use `.xcframework`, as the issue doesn't exist in those two areas.
* However some smaller frameworks, like Ada, have not yet switched. Thus we need to run this script as a workaround
* [More detailed explanation of the duplicated binaries](https://github.com/Carthage/Carthage/blob/master/Documentation/Xcode12Workaround.md)


**4. Delete the old framework in Xcode**

1. Inside Xcode, navigate to where the frameworks are kept
2. Delete old framework from this section
3. Find the framework in the Frameworks folder in the left hand navigation menu of Xcode (or find it in Finder). If the framework still exists there, delete it (move to trash)

**5. Add new framework to Xcode**

1. Drag the new framework that you downloaded from the internet into the same section as above
2. Change the option to `Do not embed`
3. Copy the new framework into the Framework directory

**6. Clean and build**



### Troubleshooting

**Xcode error: `Module compiled with Swift 5.3.1 cannot be imported by the Swift 5.4 compiler`**

* Perform the steps above (1 through 6)

**Carthage error: `Dependency "xxx" has no shared framework schemes`**

* Open the framework project in Xcode
* Up the top where you specify the simulator / device you want to run, click on `Empower` and then choose `Manage schemes`
* On the right hand side, ensure the checkboxes for "shared" are checked. If they are shared, then toggle them on and off.


**Xcode error: `Building for iOS Simulator, but the linked and embedded framework 'xFramework.framework' was built for iOS + iOS Simulator.`**

* Inside the frameworks folder
* Ensure the framework is set to `Do not embed`

