The most common type conversions are:

Objective-C Type | Swift Type
---------------- | ----------
instancetype     | N/A
id               | AnyObject
id\<Protocol\>   | Protocol
NSInteger        | Int
NSString         | String

Note that `instancetype` is only returned by Objective-C initializers.  Swift initializers do not return a value.

If the type is `(weak)` in Objective-C make it `weak` and optional in Swift.
