---
id: android-themis-doc-zh
title: Themis - 策略分发引擎
---

**策略分发引擎**提供了一套完整的策略分发服务，可以按照App版本、语言等多个维度来给不同的用户定制不同策略，支持按照流量来进行策略分发且可以保证一个用户的策略不保持不变。使用策略分发引擎可以在线上对不同的功能进行测试，帮助产品做出一个正确的决定。

https://git.toolsfdg.net/fe/bnb-android/tree/developer/libs/android-themis

#### 引入

> Binance App已经引入了**策略分发引擎**SDK，无需再次接入直接使用即可。

```groovy
implementation 'com.binance.android:themis:${lastVersion}'
```

#### 初始化

> Binance App已经完成了**策略分发引擎**SDK的初始化，无需再次初始化直接使用即可。

```kotlin
 val config = ThemisConfig(
                    appKey = "appKey",
                    appSecret = "appSecret",
                    deviceId = "deviceId",
                    uid = "uid",
                    osVersion = "osVersion",
                    appVersion = "appVersion",
                    region = "region",
                    language = "language",
                    exchangeRate = "exchangeRate"
            )
Themis.start(applicationContext, config)
```

#### 使用

获取**Flow**类型的策略

```kotlin
Themis.flow("key")
```

获取**Condition**类型的策略

```kotlin
Themis.condition("key")
```

就这么简单。



#### 自定义功能

> `Themis.start()`方法支持传入`httpClient`和`store`来自定义网络请求和数据存储。

如需自定义网络请求，可继承`HttpClient`接口，并实现`post`、`get`以及`host`方法即可。

```kotlin
interface HttpClient {

    fun host(): String

    fun <R> post(
        url: String,
        headers: Map<String, String>,
        jsonBody: String,
        parse: (String) -> Response<R>
    ): Result<Response<R>>

    fun <R> get(
        url: String,
        headers: Map<String, String>,
        parse: (String) -> Response<R>
    ): Result<Response<R>>

}
```



如需自定义数据存储，可继承`Store`,并实现`get`、`save`接口。

```kotlin
interface Store {

    fun save(key: String, value: String)

    fun get(key: String): String?

}
```
