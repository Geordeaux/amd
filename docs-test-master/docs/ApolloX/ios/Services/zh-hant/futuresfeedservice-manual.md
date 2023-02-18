---
title: FuturesFeedService 使用手冊
authors: 林政龍 (Mars Lin)
---
## 介紹

`FuturesFeedService` 用來訂閱 ApolloX 中 WebSocket 的串流資料。

## 架構圖

![FuturesFeedService Architecture](https://static.devfdg.net/image/julia/futures-feed-service-architecture.jpg)

## 使用方式

### 訂閱 (Subscribe)

訂閱 `FuturesServiceStreamName` enum 中所列的串流名稱，只需使用 `FuturesFeedService` shared instance 訂閱並傳入實作 `FuturesFeedObserving` protocol 的物件即可。

```Swift
class ExampleViewController: UIViewController {
    override func viewDidLoad() {
      super.viewDidLoad()
    
      // 1.
      FuturesFeedService.shared.subscribe(
          streamNames: [.assetIndex, .markPrice(symbol: "BTCUSDT")],
          with: self,
          forceFetch: true)
    }
}

extension ExampleViewController: FuturesFeedObserving {
    func didFetch(with content: FuturesServiceFeedContent) {
        // 2.
    }
  
    func didUpdate(with content: FuturesServiceFeedContent) {
        // 3.
    }
  
    func didRecover(streamName: FuturesServiceStreamName, result: Swift.Result<Void, StreamSubscriberRecoverError>) {
        // 4.
    }
  
    func didFail(streamName: FuturesServiceStreamName, with error: Error) {
        // 5.
    }
  
    func didConnect(streamName: FuturesServiceStreamName) {
        // 6.
    }
    
    func didDisconnect(streamName: FuturesServiceStreamName) {
        // 7.
    }
}
```

1. 讓 `observer` 訂閱串流
   * `streamNames`: 列出需訂閱的串流名稱
   * `with`: 放入實作 `FuturesFeedObserving` protocol 的物件，用來獲取 feed 的事件
   * `forceFetch`: 設定為 `true` 時，強制透過 RESTful API 取得全量資料。
2. 處理 RESTful API 來的全量資料。
3. 處理 WebSocket 的增量資料或是暫存資料。
   * 訂閱時暫存資料可能尚未存在，每次取得全量及增量後更新。
4. 當 WebSocket 連線恢復時會被呼叫。
5. 當取得全量或增量有發生問題時，會在這裡取得相對的 error 資料。
6. 可選。WebSocket 連線後呼叫。
7. 可選。WebSocket 斷線後呼叫。

### 解除訂閱 (Unsubscribe)

有兩種解除訂閱的方式：

```Swift
class ExampleViewController: UIViewController {
    override func viewDidDisapear(_ animated: Bool) {
      super.viewDidDisapear(animated)
    
      // 1.
      FuturesFeedService.shared.unsubscribe(streamNames: [.assetIndex], with: self)
    }
  
    deinit() {
      // 2.
      FuturesFeedService.shared.unsubscribeAllFeeds(with: self)
    }
}
```

1. 當需要解除一個或多個串流時使用。
2. 當需要解除所有已經訂閱的串流時使用。

### 暫停 / 恢復 (Pause / Resume)

```swift
class ExampleViewController: UIViewController {
    override func viewDidDisapear(_ animated: Bool) {
      super.viewDidDisapear(animated)
    
      // 1.
      FuturesFeedService.shared.pauseAllSubscriptions(with: self)
    }
  
    override func viewWillAppear(_ animated: Bool) {
      super.viewWillAppear(animated)
      
      // 2.
      FuturesFeedService.shared.resumeAllSubscriptions(with: self)
    }
}
```

1. 若有需要可暫停所有訂閱。相較「解除訂閱」，「暫停」不會停止訂閱但也不會收到任何事件。
   * 如果需要解除部分訂閱, 使用 `pauseSubscriptions(streamNames:with:) `。
2. 回復先前訂閱
   * 如果需要恢復部分訂閱, 使用 `resumeSubscriptions(streamNames:with:) `。

### 重取 (Refetch)

必要時可以用來重新取得全量資料：

```Swift
extension ExampleViewController {
    func pullToUpdate() {
      FuturesFeedService.shared.refetch(streamName: .assetIndex)
    }
}
```

