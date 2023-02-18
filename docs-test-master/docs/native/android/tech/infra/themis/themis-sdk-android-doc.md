---
id: android-themis-doc
title: Themis - Strategy Distribution Engine
---

**[中文](themis-sdk-android-doc-zh.md)**

The **Strategy Distribution Engine** provides a complete set of strategy distribution services, which can customize different strategies for different users according to multiple dimensions such as App version and language. It supports strategy distribution according to traffic and can ensure that a user's strategy does not remain unchanged . Using the strategy distribution engine, different functions can be tested online to help the product make a correct decision.

https://git.toolsfdg.net/fe/bnb-android/tree/developer/libs/android-themis

#### Import

> Binance App has already import the **Strategy Distribution Engine** SDK and can be used without re-import.

```groovy
implementation 'com.binance.android:themis:${lastVersion}'
```

#### Initializer

> Binance App has completed the initialization of the **Strategy Distribution Engine** SDK and can be used without re-initialization.

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

#### Usage

Gets a strategy of type **Flow**.

```kotlin
Themis.flow("key")
```

Gets a strategy of type **Condition**.

```kotlin
Themis.condition("key")
```

So easy.



#### Custom function

> The `Themis.start ()` supports incoming `httpClient` and `store` to customize network request and data store.

To customize network request, implement the `HttpClient ` and implement the`post`、`get` and `host` methods.

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

To customize data store, implement the `Store ` and implement the`get`、`save` methods.

```kotlin
interface Store {

    fun save(key: String, value: String)

    fun get(key: String): String?

}
```
