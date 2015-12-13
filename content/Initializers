Initializers declared in Swift code get exposed to Objective-C through the `Xxx-Swift.h` bridging header.  In Swift all initializers have the name `init`.  In Objective-C initializers are named (by convention) to include the name of the first parameter, e.g. `initWithHostname`.   Because of this, when the compiler generates the `Xxx-Swift.h` header, it will create an Objective-C friendly name for initializers.  So for example:

```swift
  init(hostname: String) {
    
  }
```

Will get exported as `initWithHostname` to the header file.
