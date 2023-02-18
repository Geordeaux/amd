---
id: kotlin_coroutines_for_network_requests
title: Kotlin Coroutines for network requests
hide_title: true
---

# Kotlin Coroutines for network requests

### Contracts ：stas.parshin@binance.com

Our network/API/repository layer is tightly connected with RxJava.In order to better support it for using with Coroutines there are few wrappers in `lib-base/RxCoroutines`

Basically it is suspend extension function `Observable<ResponseWrapper<T>>.await(): T`

We don't need to manually switch IO/UI threads, or use coroutines dispatchers.It should be launched from viewModelScope, lifecycleScope by simply calling `await()`

```kotlin

//Call API from ViewModel and just show result

viewModelScope.launch {
    showProgressDialog()
    RepositoryCentral.ocbsRepo.requestCoinList().await { result ->
        // update UI
    }
    hideProgressDialog()
}
```

```kotlin

/// Success and error handle

viewModelScope.launch {
    showProgressDialog()
    RepositoryCentral.ocbsRepo.requestCoinList().await(
        onSuccess = { result ->
            // update UI
        },
        onError = { error ->
            handleThrowable(error)
        })
    hideProgressDialog()
}
```

```kotlin
// await() without lambda returns safe BncResponse to avoid try-catch

viewModelScope.launch {
    val response: BncResponse<OcbsNewSellSelectorBean>? = RepositoryCentral.ocbsRepo.getOcbsNewSellSelector()?.await()
    if (response?.result != null) {
        // show UI
    }
    if (response?.error != null) {
        // handle error
    }
     
    // OR using
    response?.onSuccess { result ->
    }
    response?.onError { error ->
    }
    response?.onEmpty {
    }
}
```

```kotlin
// awaitThrows() for advanced usage, async requests, should be try-catched

viewModelScope.launch {
    supervisorScope {
        // these requests will run in parallel
        val sellResponse = async { RepositoryCentral.ocbsRepo.getOcbsNewSellSelector()?.awaitThrows() }
        val buyResponse = async { RepositoryCentral.ocbsRepo.getOcbsNewBuySelector()?.awaitThrows() }
        try {
            val ocbsSell: OcbsNewSellSelectorBean? = sellResponse.await()
            val ocbsBuy: OcbsNewBuySelectorBean? = buyResponse.await()
            // show 2 responses
        } catch (e: Throwable) {
            // show error
        }
    }
}
```