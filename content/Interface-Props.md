Convert the following:

```obj-c
@interface ImagePathHelper ()

@property (nonatomic, retain) NSString* domain;

@end
```

into:

```swift
	let domain: String
```

Alternatively, use `var` if the property will be modified outside of the initializer.  You can also make the property `private` or `internal` if desired.
