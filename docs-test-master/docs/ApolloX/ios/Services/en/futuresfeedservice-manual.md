---
title: FuturesFeedService Manual
authors: Zhen Long, Lin (Mars Lin)
---
## Introduction

`FuturesFeedService` is used to observe the stream data via WebSocket in ApolloX.

## Architecture

![FuturesFeedService Architecture](https://static.devfdg.net/image/julia/futures-feed-service-architecture.jpg)

## How to use?

### Subscribe

To subscribe the stream data of some of the stream names listed in the enum `FuturesServiceStreamName`, just need to subscribe with `FuturesFeedService` shared instance and conform to the `FuturesFeedObserving` protocol.

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

1. Subscribe streams with observer
   * `streamNames`: List the stream names to subscribe
   * `with`: The observer conformed to `FuturesFeedObserving` protocol to receive feed events
   * `forceFetch`: Force to fetch the complete data via RESTful API if `true`
2. Handle the complete data which is returned from feed `fetchContent` via RESTful API.
3. Handle the stream data via WebSocket or cached data.
   * The cached data will be existed and updated when receiving the complete data via RESTFul API or the stream data via WebSocket.
4. This function will be called when the WebSocket connection is recovered.
5. Get error if something went wrong when fetching content or receiving updates.
6. Optional. This function will be called when the WebSocket connection is connected.
7. Optional. This function will be called when the WebSocket connection is disconnected. 

### Unsubscribe

There are two ways to unsubscribe streams:

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

1. The way to unsubscribe one or some of the stream names.
2. The way to unsubscribe all of the stream names which are subscribed before by the observer (`self`).

### Pause / Resume

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

1. Pause all subscriptions if need. Comparing with `unsubscribeAllFeeds`, pause the subscriptions will not unsubscribe the feeds and not receive any events as well.
   * If want to pause partial subscriptions, use `pauseSubscriptions(streamNames:with:) `.
2. Resume all the subscriptions if the observer has subscribed before.
   * If want to resume partial subscriptions, use `resumeSubscriptions(streamNames:with:) `

### Refetch

Refetch the complete data (via RESTful API) when needed:

```Swift
extension ExampleViewController {
    func pullToUpdate() {
      FuturesFeedService.shared.refetch(streamName: .assetIndex)
    }
}
```

