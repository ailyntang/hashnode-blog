## Understanding binaries when archiving a build - WIP

Steps in releasing an app
1. Build on your local machine with Xcode
 - You need to select a specific device when doing this
 - The build is compiled for this device and its CPU / architecture (e.g. ARM 64 bit, Intel x86 64 bit, ARM 32 bit etc)

2. Archive a build
  - There are three components to archiving a build
    - Create the required binaries
    - Signing keys
    - Provisioning profiles

Let's talk just about creating the required binaries.

Whenever Xcode compiles and builds, it is compiling the language into binary.
It goes from `interpretive language ` (e.g. Swift) > `machine code` > `binary`
* Native code sits somewhere in this spectrum.. not sure where. Maybe at machine code?

Binaries are tailored to the device's CPU. We need a different binary for each different CPU type. Why?
* Native code is read differently


### Frameworks / Libraries / SDKs
If any of these are written in native code (aka C, C#, C++) then the SDK must provide different pre-compiled versions for each of the supported CPUs.

If the SDK doesn't support a CPU, then if you import that library into your code, you will not be able to support that CPU either.






### Digression into CPU types
Common CPU types (also called architecture)
* ARM 32 bit (ARM is a company, like Intel is a company)
* ARM 64 bit
* x86 64 bit (Intel)

Some background on ARM and Intel:
* All Android devices were and are built on ARM
* iOS was built on Intel in the early days. At some stage, maybe iPhone 5 or 6, Apple switched to ARM as ARM is believed to be faster for mobile devices

Some background on 32 vs 64 bits
* 64 bits can run 32 bits. However Apple have stopped making 64 bits backward compatible as a design choice for some of the laptops, ie M1
* 32 bits cannot run 64 bits
  * Think of it this way: a 32 bit integer has less information than a 64 bit integer. So a 64 bit can load a 32 bit integer with no issues.
  * However a 32 bit integer cannot load a 64 bit integer.



### IPA
* All files you need to run an appilcation
* Inside the binary there is a full file structure like images, code, icons
* Is an IPA a binary then?


  - This ensures that the build is compiled for all possible devices. Different devices can have different architectures.
  - 

### Raw notes

Binary
- [ ] 0s and 1s
- [ ] A file that’s no longer machine readable, and now it’s machine language
- [ ] Compiling the language into a binary

Build vs app store ready
- [ ] Build is for one specific device
- [ ] Different devices have different CPU architecture
    - [ ] 64 bit
    - [ ] 32 
    - [ ] ARM - different way of doing CPU: ARM64 apple is the apple 64 bit architectyure. ARM is a seperate company that Apple have chosen to use over Intel now. Theoretically ARM is optimised for mobile, and mobile phones have used ARM for a long time. Android have always used ARM, iOS used Intel until iphone 5 or 6 then switched.
    - [ ] Intel: x86_64 is the Intel 64 bit architecture
    - [ ] 
    - [ ] So how the logic inside the board works and how it interprets commands





Librairies are pre-compiled and specifyt eharchitecture they support


Unit tests are run on local machine, not through the simulator



Different architecturs in fraemworks are only important for C code / C# / C++. Anything that comiples to native code.
- [ ] With C, need to have compiled versio for te the CPU
    - [ ] C code is called NATIVE to the CPU. It’s not just C code that is native to the CPU. BUT C is often the same.
    - [ ] C is the language you write that copmiles to native code
    - [ ] 
- [ ] Wihtout C code


Java run time
- [ ] run time is the native code interpretaion
- [ ] Java doesn’t need to comiple down to native code because it runs in runtime
- [ ] Run time deals with the cpu



If any libraries use native code (vs interprative languages), they will need to specifiy the different architecutres
- [ ] Native code (C, C#, C++) is much faster than interprative languages (e.g. swift)
- [ ] 



Binary
Next step up is machine code
