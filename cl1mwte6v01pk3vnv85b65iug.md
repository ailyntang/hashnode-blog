## Debugging `error: Abort trap: 6 (in target 'x' from project 'x')`

At the very top of the stack trace, it will have something like:

`4.	Running pass 'Module Verifier' on function '@"$s10DeviceRisk0aB7ManagerC7E8sendData33_C42FBD2CD0F947D2F06CD9148B087CF7LL6apiKey7sources12existingUUID11userConsent8delegateySS_SayAA0abF7SourcesOGSSSgAD06SocureaB7ServiceC04UserV0OAD0aB8DelegateAFLLCtF"'`

Look past all the weirdness, and what the stack trace is trying to say is:
* The swift compiler is trying to do a bunch of low level things to compile the code
* One of these low level things is failing
* It is failing on the function `sendData`
  * The function name is bolded
  * The other words in that mess are the arguments in that function, e.g. `apiKey`, `existingUUID`, `userConsent`, etc.


Here's the full stack trace
```
4.	Running pass 'Module Verifier' on function '@"$s10DeviceRisk0aB7ManagerC7E8sendData33_C42FBD2CD0F947D2F06CD9148B087CF7LL6apiKey7sources12existingUUID11userConsent8delegateySS_SayAA0abF7SourcesOGSSSgAD06SocureaB7ServiceC04UserV0OAD0aB8DelegateAFLLCtF"'
Stack dump without symbol names (ensure you have llvm-symbolizer in your PATH or set the environment var `LLVM_SYMBOLIZER_PATH` to point to it):
0  swift-frontend           0x000000010facc667 llvm::sys::PrintStackTrace(llvm::raw_ostream&, int) + 39
1  swift-frontend           0x000000010facb5f8 llvm::sys::RunSignalHandlers() + 248
2  swift-frontend           0x000000010faccc76 SignalHandler(int) + 278
3  libsystem_platform.dylib 0x00007ff8028f6dfd _sigtramp + 29
4  libsystem_platform.dylib 0x0000000000000003 _sigtramp + 18446603370537980451
5  libsystem_c.dylib        0x00007ff80282cd24 abort + 123
6  swift-frontend           0x000000010abfa452 swift::performFrontend(llvm::ArrayRef<char const*>, char const*, void*, swift::FrontendObserver*)::$_2::__invoke(void*, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, bool) + 978
7  swift-frontend           0x000000010fa15dee llvm::report_fatal_error(llvm::Twine const&, bool) + 286
8  swift-frontend           0x000000010fa15ccb llvm::report_fatal_error(char const*, bool) + 43
9  swift-frontend           0x000000010f9ac8af (anonymous namespace)::VerifierLegacyPass::runOnFunction(llvm::Function&) + 111
10 swift-frontend           0x000000010f946aba llvm::FPPassManager::runOnFunction(llvm::Function&) + 1354
11 swift-frontend           0x000000010f945da1 llvm::legacy::FunctionPassManagerImpl::run(llvm::Function&) + 113
12 swift-frontend           0x000000010f94d2c5 llvm::legacy::FunctionPassManager::run(llvm::Function&) + 341
13 swift-frontend           0x000000010b0ddf11 swift::performLLVMOptimizations(swift::IRGenOptions const&, llvm::Module*, llvm::TargetMachine*) + 1585
14 swift-frontend           0x000000010b0ded77 swift::performLLVM(swift::IRGenOptions const&, swift::DiagnosticEngine&, llvm::sys::SmartMutex<false>*, llvm::GlobalVariable*, llvm::Module*, llvm::TargetMachine*, llvm::StringRef, swift::UnifiedStatsReporter*) + 2055
15 swift-frontend           0x000000010ac05bf9 performCompileStepsPostSILGen(swift::CompilerInstance&, std::__1::unique_ptr<swift::SILModule, std::__1::default_delete<swift::SILModule> >, llvm::PointerUnion<swift::ModuleDecl*, swift::SourceFile*>, swift::PrimarySpecificPaths const&, int&, swift::FrontendObserver*) + 3529
16 swift-frontend           0x000000010abf7296 swift::performFrontend(llvm::ArrayRef<char const*>, char const*, void*, swift::FrontendObserver*) + 13510
17 swift-frontend           0x000000010ab38f78 main + 1032
18 dyld                     0x000000011c04551e start + 462
error: Abort trap: 6 (in target 'x' from project 'x')
```