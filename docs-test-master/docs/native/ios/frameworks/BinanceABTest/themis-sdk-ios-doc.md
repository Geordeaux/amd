---
id: ios-themis-doc
title: Themis - Strategy Distribution Engine
---

**[中文](themis-sdk-ios-doc-zh.md)**

The **Strategy Distribution Engine** provides a complete set of strategy distribution services, which can customize different strategies for different users according to multiple dimensions such as App version and language. 

It supports strategy distribution according to traffic and can ensure that a user's strategy does not remain unchanged . Using the strategy distribution engine, different functions can be tested online to help the product make a correct decision.

The source code of the project is [here](https://git.toolsfdg.net/fe/BinanceABTest). For access support, please contact harry.duan@binance.com.

### Import

> Binance App has added the **Themis** SDK, namely `BinanceABTest`, so you can use it directly without reconnecting.

We can import it in `Podfile` in the following way.

```swift
pod 'BinanceABTest'
```
Since `BinanceABTest` is only released in `BNCSpecs` Source at present, it is necessary to introduce `BNCSpecs` source in `Podfile`.

```ruby
source 'https://git.toolsfdg.net/fe/BNCSpecs.git'
```

### Initialize

> Binance App has completed the initialization of the **Themis** SDK, you can use it directly without re-initialization.

```swift
import BinanceABTest

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        // create Config
        let config = BinanceABTestConfig(
            appKey: "1037751243", // mapping `APP_Key` in SDE platform
            appSecret: "IWYi1p6HHJj0wTjTOWX451kqp7LpF5ck", // mapping `APP_SECRET` in SDE platform
            UID: "TEST_UID" // Use the compliant User's unique ID, which is `bnc_uuid` in Binance App
        )
        
        // Start Themis
        Themis.start(config)
        
        return true
    }
}
```

## Usage

### `Flow` strategy

Get the `Flow` type strategy:

```swift
Themis.flow("key")
```

### `Condition` strategy

Get the `Condition` type strategy:

```swift
Themis.condition("key")
```


## Adaptation

### Persistence

Because of the different persistence schemes used by different apps. Without any settings by default, **Themis SDK will use iOS native UserDefault for Key Value persistence management**.

In Binance App, the `mmkv` persistence scheme is used globally. At this time, we can add the implementation `class` of `BinanceABTestCacheResolver` to complete the change of the persistence management scheme:

```swift
class BinanceMMKVThemisCacheResolver: BinanceABTestCacheResolver {
    
    static let shared = BinanceMMKVThemisCacheResolver()
    
    func store(key: BinanceABTestCache.Key, value: String?) {
        KVStore.shared.set(value, forKey: key.rawValue)
    }
    
    func get(key: BinanceABTestCache.Key) -> String? {
        return KVStore.shared.string(forKey: key.rawValue)
    }
}

```

Here we can bring this `BinanceABTestCacheResolver` when calling the `Themis.start` method:

```swift
Themis.start(
    config,
    resolver: BinanceMMKVThemisCacheResolver.shared
)
```


### Dynamic Domain 

There may be differences in the services requested by different apps. Currently Dynamic Domain is used in Binance App, so the following configuration will be used in Binance App:

```swift
let config = BinanceABTestConfig(
    appKey: "1037751243", // mapping `APP_Key` in SDE platform
    appSecret: "IWYi1p6HHJj0wTjTOWX451kqp7LpF5ck", // mapping `APP_SECRET` in SDE platform
    UID: "TEST_UID" // Use the compliant User's unique ID, which is `bnc_uuid` in Binance App
)

config.setCustomDomainGet {
    // A getter function，keep update
    return BinanceClient.endpoint.currentURL.host
}
```





