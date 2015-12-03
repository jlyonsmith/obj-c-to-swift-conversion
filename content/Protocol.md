Convert as follows:

```obj-c
@protocol ImagePathHelperProtocol
	- (NSURL*)buildUrlForImage:(id<ImageProtocol>)image sizeType:(ImageSize)sizeType;
	- (NSURL*)buildUrlForImagePath:(NSString*)path withHash:(NSString*)hash sizeType:(ImageSize)sizeType;
	- (NSString*)getSizeString:(ImageSize)sizeType;
@end
```

becomes:

```swift
@objc protocol RSImagePathHelperProtocol {
    func buildUrl(image: ImageProtocol, sizeType: RSImageSize) -> NSURL
    func buildUrl(imagePath path: String, withHash hash: String, sizeType: RSImageSize) -> NSURL
    func getSizeString(sizeType: RSImageSize) -> NSString
}
```
