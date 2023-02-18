---
id: ios-themis-doc-zh
title: Themis - 策略分发引擎
---

## SDE 策略分发引擎

**Themis 策略分发引擎(Strategy Distribution Engine，下文称 Themis)** 提供了一套完整的策略分发服务，可以按照 App 版本、语言等多个维度来给不同的用户定制不同策略，支持按照流量来进行策略分发且可以保证一个用户的策略不保持不变。


使用策略分发引擎可以在线上对不同的功能进行测试，帮助产品做出一个正确的决定。

项目源码在[这里](https://git.toolsfdg.net/fe/BinanceABTest)，接入支持请联系 harry.duan@binance.com。


## 使用 CocoaPods 集成

> Binance App 已经加入了 **Themis** SDK，即 `BinanceABTest`，无需再次接入直接使用即可。

我们可以在 `Podfile` 中通过以下方式引入。

```ruby
pod 'BinanceABTest'
```

由于目前 `BinanceABTest` 只发布在 `BNCSpecs` Source 中，所以在 `Podfile` 中需要引入 `BNCSpecs` 源。

```ruby
source 'https://git.toolsfdg.net/fe/BNCSpecs.git'
```

#### 初始化

> Binance App 已经完成了**Themis** SDK 的初始化，无需再次初始化直接使用即可。

```swift
import BinanceABTest

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        // 创建配置
        let config = BinanceABTestConfig(
            appKey: "1037751243", // 在官方平台上提供对 APP_Key
            appSecret: "IWYi1p6HHJj0wTjTOWX451kqp7LpF5ck", // 在官方平台上提供对 APP_SECRET
            UID: "TEST_UID" // 使用合规后的 User 唯一标识，Binance App 中为 bnc_uuid
        )
        
        // 启动 SDK
        Themis.start(config)
        
        return true
    }
}
```

## 使用

### Flow 类型的策略获取

```swift
Themis.flow("key")
```

### Condition 类型的策略获取

```swift
Themis.condition("key")
```


## 功能定制

### 持久化方案适配

由于不同 App 使用的不同持久化方案。在默认不进行任何设置的情况下，**Themis SDK 会使用 iOS 原生 UserDefault 进行 Key Value 持久化管理**。

在 Binance App 中，全局使用的是 `mmkv` 持久化方案，此时我们可以增加 `BinanceABTestCacheResolver` 的实现 `class` 来完成持久化管理方案的更改：

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

这里我们在调用 `Themis.start` 方法的时候，带上这个 `BinanceABTestCacheResolver` 即可：

```swift
Themis.start(
    config,
    resolver: BinanceMMKVThemisCacheResolver.shared
)
```


### Domain 动态获取

由于不同 App 所请求的服务可能存在差异。目前 Binance App 中使用了 Dynamic Domain，所以在 Binance App 中会使用以下的配置：

```swift
let config = BinanceABTestConfig(
    appKey: "1037751243", // 在官方平台上提供对 APP_Key
    appSecret: "IWYi1p6HHJj0wTjTOWX451kqp7LpF5ck", // 在官方平台上提供对 APP_SECRET
    UID: "TEST_UID" // 使用合规后的 User 唯一标识，Binance App 中为 bnc_uuid
)

config.setCustomDomainGet {
    // 这里用来获取 dynamic domain
    // 传入的是一个 Get 方法，所以每一次刷新都可以被取到
    return BinanceClient.endpoint.currentURL.host
}
```





