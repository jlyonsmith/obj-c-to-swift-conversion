This is where the majority of the conversion work takes place.  As an example, take the following:

```obj-c
@implementation RSImagePathHelper

// Preferred constructor:
- (instancetype)initWithUserDefaults:(NSUserDefaults*)userDefaults
{
    NSString *domain = [RSBackendApi urls].assetsUrl;
    
    return [self initWithDomain:domain];
}

- (instancetype)initWithDomain:(NSString*)domain
{
    self = [super init];
    if (self) {
        self.domain = domain;
    }
    return self;
}

- (NSURL*)buildUrlForImage:(id<ImageProtocol>)image sizeType:(RSImageSize)sizeType
{
    if (image) {
        @try {
            NSString* sizeString = [self getSizeString:sizeType];
            NSString* imagePath = [image.path stringByReplacingOccurrencesOfString:@"files" withString:image.hashName];
            NSURL* url = [NSURL URLWithString:[[NSString stringWithFormat:@"%@/%@/%@", self.domain, sizeString, imagePath] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding]];
            return url;
        }
        @catch (NSException* exception) {
            RSLOG(@"%@", exception);
        }
    }

    return nil;
}

- (NSURL*)buildUrlForImagePath:(NSString*)path withHash:(NSString*)hash sizeType:(RSImageSize)sizeType
{
    if (path) {
        @try {
            NSString* sizeString = [self getSizeString:sizeType];
            NSString* imagePath = [path stringByReplacingOccurrencesOfString:@"files" withString:hash];
            NSURL* url = [NSURL URLWithString:[[NSString stringWithFormat:@"%@/%@/%@", self.domain, sizeString, imagePath] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding]];
            return url;
        }
        @catch (NSException* exception) {
            RSLOG(@"%@", exception);
        }
    }

    return nil;
}

- (NSString*)getSizeString:(RSImageSize)sizeType
{
    switch (sizeType) {
    case RSImageSizeFull:
        return @"full";
    case RSImageSizeTiny:
        return @"tiny";
    case RSImageSizeMicro:
        return @"micro";
    case RSImageSizeSmall:
        return @"small";
    case RSImageSizeHuge:
        return @"1080";
    }
}

@end
```

One way to convert this is as follows:

```swift
import Foundation // 1

@objc class ImagePathHelper : NSObject, ImagePathHelperProtocol {
    internal var domain: String!
    internal var formCharacterSet: NSMutableCharacterSet!
    
    private override init() { // 2(a)
        super.init()
    }
    
    init(domain: String) { // 2(b)
        super.init()
        self.domain = domain
        
        let charSet = NSMutableCharacterSet.alphanumericCharacterSet()

        charSet.addCharactersInString("-._* ")
        self.formCharacterSet = charSet
    }
    
    // 3
    func stringByAddingPercentEncodingForFormData(fromString: String) -> String? {
        return fromString.stringByAddingPercentEncodingWithAllowedCharacters(self.formCharacterSet)?.stringByReplacingOccurrencesOfString(" ", withString: "+")
    }
    
    // 4(a)
    func buildUrlForImage(image: ImageProtocol?, sizeType: RSImageSize) -> NSURL? {
        var url: NSURL?
        
        if let safeImage = image {
            let sizeString = self.getSizeString(sizeType)
            let imagePathWithFiles = safeImage.path.stringByReplacingOccurrencesOfString("files", withString: safeImage.hashName)
            if let fullImagePath = stringByAddingPercentEncodingForFormData("\(self.domain)/\(sizeString)/\(imagePathWithFiles)") {
                url = NSURL(fileURLWithPath: fullImagePath)
            }
        }
        
        return url
    }
    
    // 4(b)
    func buildUrlForImagePath(imagePath: String?, withHash hash: String, sizeType: RSImageSize) -> NSURL? {
        var url: NSURL?
    
        if let safeImagePath = imagePath {
            let sizeString = self.getSizeString(sizeType)
            let imagePathWithFiles = safeImagePath.stringByReplacingOccurrencesOfString("files", withString: hash)
            
            if let fullImagePath = stringByAddingPercentEncodingForFormData("\(self.domain)/\(sizeString)/\(imagePathWithFiles)") {
                url = NSURL(fileURLWithPath: fullImagePath)
            }
        }
        
        return url
    }
    
    // 5
    func getSizeString(sizeType: ImageSize) -> String {
        switch (sizeType) {
        case .Full:
            return "full"
        case .Tiny:
            return "tiny"
        case .Micro:
            return "micro"
        case .Small:
            return "small"
        case .Huge:
            return "1080"
        }
    }
}
```

Note that this includes both protocol, interface and property code from above.  There is quite a lot going on here so lets break it down.

1. The `import` declaration replaces the pre-processor `#include` directives.  This brings symbols from the `Foundation`  library.  Symbols for the current project are automatically imported.
2. (a) Here we are hiding the `init` method from `NSObject` for classes that derived from this one with the `private` modifier.  Sometimes you want to do this for dependency injection frameworks, for example.  (b) Here we set the instance properties after calling the super class (remember, Swift objects force two phase initialization)
3. Refactoring.  Here we are doing some refactoring during the conversion.  `stringByAddingPercentEscapesUsingEncoding` is deprecated in favor of `stringByAddingPercentEncodingWithAllowedCharacters`, so we take the opportunity to change the implementation a bit.  You have tests to catch any problems, right?
4. These two methods were using `NSException` to catch errors from either null references or other conversion failurs.  In Swift the preference is to favor using optionals and `try/catch` (which is `NSError` based, _not_ `NSException` based)  Note the use of optional return types too so that the method can continue to return `nil` if it cannot do what it is supposed to.  A _much_ preferred solution here would be to change the method to `throws` and return errors that way, but that would require changing all the call sites.  The goal in the first phase of conversion is to get _working_ Swift code, we can make it idiomatic later.
5. This method is the most straightforward.  No surprises here.
