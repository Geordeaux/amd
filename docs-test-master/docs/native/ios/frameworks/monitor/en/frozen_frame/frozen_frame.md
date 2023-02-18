---
id: frozen_frame
title: Frozen Frame
hide_title: true
---

# Frozen Frame 

## Background

The definition of the *Frozen Frame*  given in Google's official document is as follows：

>  Percentage of screen instances during which more than 0.1% of frames took longer than 700 ms to render.

That is: **For a specified page, if there is a rendering time of more than 700ms that accounts for the proportion of the total dwell time, more than 0.1%** is defined as a *Frozen Frame* event.

Compared with the *Slow Rendering* event, *Frozen Frame* exposes the problem of the page. *Slow Rendering* is a sampling test in terms of user perception. And *Frozen Frame* is an active detection mechanism. So in two dimensions, to reflect the stuck problem of the corresponding page.

## Field description

| Property | Type | Const Value | Note |
| --- | --- | --- | --- |
| t  | MonitorType | FROZEN-FRAME | Tracker Type |
| ct | long |  |  Single page dwell time(ms) |
| at  | long |  |  *Frozen Frame* Time(ms) |
| p | string |  |  ViewController Name |

## Strategy

Use RunLoop's listener mode to obtain the rendering time of one frame. **If a rendering exceeds 700ms, it will be reported**. Calculate the ratio of *Frozen Frame* by calculating the quotient of Frozen time and user stay time.

## Polaris Score Calculator

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f1.png)


## JSON Introduction

```json
{
  "_index": "events-track-2021-01-21",
  "_type": "doc",
  "_id": "49614679838409895680918106858172456943313319932767244050.0",
  "_version": 1,
  "_score": null,
  "_source": {
    "ipaMode": "RELEASE",
    "ct": 972.4202156066895,
    "d": 1613111170.010251,
    "at": 0,
    "p": "Binance.FundsViewController",
    "ci": "41.2",
    "wifi": false,
    "t": "FROZEN-FRAME",
    "bnc-uuid": "20E15F06-2F66-4D84-A793-366EAA755204",
    "versionCode": "2.24.0",
    "clienttype": "ios",
    "dt": "1970-01-19 16:05:11.170",
    "system_timestamp": 1611211004507,
    "request_ip": " 64.252.180.96",
    "request_ip0": "109.37.133.40",
    "request_country": "Netherlands",
    "request_city": "Venlo",
    "system_dt": "2021-01-21 06:36:44.507",
    "network": "vf nl4G",
    "system-version": "14.3",
    "brand": "iPhone 11 Pro Max",
    "log_type": "api"
  },
  "fields": {
    "dt": [
      "1970-01-19T16:05:11.170Z"
    ],
    "system_dt": [
      "2021-01-21T06:36:44.507Z"
    ]
  },
  "highlight": {
    "t": [
      "@kibana-highlighted-field@FROZEN@/kibana-highlighted-field@-FRAME"
    ]
  },
  "sort": [
    1613111170
  ]
}
```

## LifeCycleChecker Module

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f2.png)

`LifeCycleChecker` is an object that every VC should hold. This object will be injected into common life cycle methods and custom symbol methods in VC.

When we need to complete a life cycle monitoring requirement, we can construct a corresponding `Trace` that conforms to the `LifeCycleTrace` protocol, and implement the `create(sceneName:sceneType:) -> Self?` method to complete the adaptor injection .

## What's is Trace ？

Let's use First Frame as an example. First, we will create a `FirstFrameLifeCycleTrace` and follow the `LifeCycleTrace` protocol to implement the prescribed method. Among them, the definition of `LifeCycleTrace` is as follows:

```swift
public protocol LifeCycleTrace {
    
    /// Page Name
    var sceneName: String { get }
    
    var occasionRule: LifeCycleResolveRules { get set }
    
    /// Selector To Occasion
    /// The Configuration about selector mapping
    /// - Parameter sel: default #function
    func selectorResolve(sel: String) -> LifeCycleTraceOccasion
    
    /// Occastion Callback
    mutating func startActionHandler(occasion: LifeCycleTraceOccasion, state: LifeCycleTraceState)
    
    mutating func activeActionHandler(occasion: LifeCycleTraceOccasion, state: LifeCycleTraceState)
    
    mutating func finishActionHandler(occasion: LifeCycleTraceOccasion, state: LifeCycleTraceState)
    
    /// Adapter Create
    /// - Parameters:
    ///   - sceneName: page name
    static func create(sceneName: String) -> Self?
    
    mutating func setValue(_ value: String, forAttribute: String)
    
    mutating func removeAttribute(_ attribute: String)
}
```

Here we can see that **Trace is actually a description of a monitoring event. The callbacks for three timings of start (start), trigger (active) and end (finish) are defined here. **

First Frame is actually recording the time `viewDidAppear` from the first loadView of a VC to the completion of rendering. So correspondingly, `FirstFrameLifeCycleTrace` can be implemented as follows:

```swift
public struct FirstFrameLifeCycleTrace: LifeCycleTrace {
    
    static var stopHandler: (String, TimeInterval) -> Void = { _, _ in }
    
    public var sceneName: String
    
    public var occasionRule: LifeCycleResolveRules

    init(sceneName: String,
         occasionRule: LifeCycleResolveRules) {
        self.sceneName = sceneName
        self.occasionRule = occasionRule
    }
    
    public func finishActionHandler(occasion: LifeCycleTraceOccasion, state: LifeCycleTraceState) {
        if case let LifeCycleTraceState.finish(_,_, tot) = state {
            BinanceLog.log(category: .firstFrame, message: "Function [\(self.sceneName)] from [\(self.occasionRule.from(sceneName).rawValue)] to [\(self.occasionRule.to(sceneName).rawValue)] cost => \(tot)s")
            DispatchQueue.global().async {
                FirstFrameLifeCycleTrace.stopHandler(self.sceneName, tot)
            }
        }
    }
    
    public static func create(sceneName: String) -> FirstFrameLifeCycleTrace? {
        let firstFrameRules = LifeCycleResolveRules.factory.createFirstFrameRule()
        return FirstFrameLifeCycleTrace(sceneName: sceneName,
                                        occasionRule: firstFrameRules)
    }
}

// This is an initialization Rules Factory, here only the factory method of First Frame is released
public class LifeCycleResolveRules {
	public class factory {
        /// Create the First Frame Rule
        /// - Returns: First Frame Rules
        public static func createFirstFrameRule() -> LifeCycleResolveRules {
            // TODO: from apollo
            let defaultRule = LifeCycleResolveRule(defaultFrom: .loadView,
                                                   defaultTo: .viewDidAppear)
            let defaultRules = LifeCycleResolveRules(defaultRule)
                .appendLazy("BNCFuturesModule.FutureTradeViewController")
                .appendLazy("BNCFuturesModule.QuarterlyTradeViewController")
                .appendLazy("Binance.FundsViewController")
                .appendLazy("Binance.MarginFundsViewController")
                .appendLazy("Binance.FiatFundsViewController")
            return defaultRules
        }
    }
}
```

## The relationship between `LifeCycleTrace` and `LifeCycleTraceSession`

`LifeCycleTraceSession` is the carrier of `LifeCycleTrace`, and each `LifeCycleTraceSession` will hold a `LifeCycleTrace`.

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f3.png)

Through the create method of LifeCycleTrace, a corresponding LifeCycleTraceSession will be generated and added to the current LifeCycleChecker. Then when the VC triggers the method in the corresponding rule, it will traverse all LifeCycleTraceSession, and respond to the callback corresponding to the trace according to the internally encapsulated rules.
