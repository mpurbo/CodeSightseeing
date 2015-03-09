# ReactiveCocoa

## Reactify delegates

By using [`rac_signalForSelector:fromProtocol:`](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/v2.4.7/ReactiveCocoa/NSObject%2BRACSelectorSignal.m#L323) we can easily change delegate calls to signals. 

Nothing interesting in the implementation itself:
```objc
- (RACSignal *)rac_signalForSelector:(SEL)selector fromProtocol:(Protocol *)protocol {
    NSCParameterAssert(selector != NULL);
    NSCParameterAssert(protocol != NULL);

    return NSObjectRACSignalForSelector(self, selector, protocol);
}
```
But things get interesting at [`NSObjectRACSignalForSelector`](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/v2.4.7/ReactiveCocoa/NSObject%2BRACSelectorSignal.m#L175)






