In general you can skip the interface definition and just convert the  implementation.  

One important area to note is with initializers.  If you have an Objective-C class that has been modified to be used by Swift it may have had the `NS_UNAVAILABLE` or `NS_DESIGNATED_INITIALIZER` macros added to it.  These correspond to the designated initializer concept in Swift so you should preserve them:

```obj-c
@interface ImagePathHelper : NSObject <ImagePathHelperProtocol>

- (instancetype)init NS_UNAVAILABLE;
- (instancetype)initWithDomain:(NSString*)domain NS_DESIGNATED_INITIALIZER;
- (NSURL*)buildUrlForImage:(id<ImageProtocol>)image sizeType:(ImageSize)sizeType;
- (NSURL*)buildUrlForImagePath:(NSString*)path withHash:(NSString*)hash sizeType:(ImageSize)sizeType;
- (NSString*)getSizeString:(ImageSize)sizeType;

@end
```
Becomes:

```swift
@objc class RSImagePathHelper : NSObject, RSImagePathHelperProtocol {
    internal init() { }
    init(domain: String)
    func buildUrl(image image: ImageProtocol, sizeType: RSImageSize) -> NSURL
    func buildUrl(imagePath imagePath path: String, withHash hash: String, sizeType: RSImageSize) -> NSURL
    func getSizeString(sizeType: RSImageSize) -> String
}
```

Note the following:

- That initializers are not functions, although they look them, so no need for the `func` statement.  Also, you can make a designated  initializer publicly unavailable by marking it `private` or `internal`.

- The `ForImage` and `ForImagePath` were replaced with named external parameters by specifying the 
