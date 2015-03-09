# ReactiveCocoa

## Reactify delegates

By using [`rac_signalForSelector:fromProtocol:`](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/v2.4.7/ReactiveCocoa/NSObject%2BRACSelectorSignal.m#L323) we can easily forward delegate calls to signals. 

Nothing interesting in the method itself:
```objc
- (RACSignal *)rac_signalForSelector:(SEL)selector fromProtocol:(Protocol *)protocol {
    NSCParameterAssert(selector != NULL);
    NSCParameterAssert(protocol != NULL);

    return NSObjectRACSignalForSelector(self, selector, protocol);
}
```
But things get mildly interesting at [`NSObjectRACSignalForSelector`](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/v2.4.7/ReactiveCocoa/NSObject%2BRACSelectorSignal.m#L175). 
```objc
static RACSignal *NSObjectRACSignalForSelector(NSObject *self, SEL selector, Protocol *protocol) {
	SEL aliasSelector = RACAliasForSelector(selector);

	@synchronized (self) {
		RACSubject *subject = objc_getAssociatedObject(self, aliasSelector);
		if (subject != nil) return subject;
        ....
    }
}
```
It starts with [`RACAliasForSelector`](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/v2.4.7/ReactiveCocoa/NSObject%2BRACSelectorSignal.m#L239) that is used to generate an alias for the selector initially passed. Nothing amazing in there
```objc
static SEL RACAliasForSelector(SEL originalSelector) {
	NSString *selectorName = NSStringFromSelector(originalSelector);
	return NSSelectorFromString([RACSignalForSelectorAliasPrefix stringByAppendingString:selectorName]);
}
```
but I did learned a couple of things:
- [`NSStringFromSelector`](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Miscellaneous/Foundation_Functions/#//apple_ref/c/func/NSStringFromSelector). Never had to use this before.
- [`RACSignalForSelectorAliasPrefix`](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/v2.4.7/ReactiveCocoa/NSObject%2BRACSelectorSignal.m#L25). Nothing critical, but this reminds me that I shouldn't use macro for something like this.

A global `RACSubject` then will be assigned to produce signal for the selector originally passed.



## Stream naming








