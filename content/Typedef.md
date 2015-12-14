Convert as follows:

```obj-c
enum NS_ENUM(NSInteger, MyEnumType) {
    MyEnumTypeMicro,
    MyEnumTypeTiny,
    MyEnumTypeSmall,
    MyEnumTypeFull
};
```

becomes:

```swift
@objc enum ImageSizeType: Int {
    case Micro
    case Tiny
    case Small
    case Full
    case Huge
}
```

In Objective-C the enumerations will be available under their old names, as Swift bridges them as the enum name prefixed by the enum type name.

The _raw value_ of the Swift enumeration is exposed to Objective-C, but only integer and floating point types can be used. 
