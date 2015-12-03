Typically, iOS code includes 

```obj-c
#import <Foundation/Foundation.h>
```

Which is roughly equivalent to:

```swift
import Foundation
```

One of the key things about Swift is that a single file is used for both exporting and implementation of classes.  So your Objective-C file is going to pretty much be irrelevent.  You can pretty much just copy your `.m` file and convert that to Swift.

This means you can copy the `.m` file over to the `.swift` file and use that as the framework for conversion.  However, it is common for `.h` file to include enumerations and protocols, so you may need to copy those into the `.swift` file as well.

### Method Signatures

The first place where you will spend the majority of your conversion typing time is with converting method signatures to their Swift equivalents.  Just about everything is different in Swift.
