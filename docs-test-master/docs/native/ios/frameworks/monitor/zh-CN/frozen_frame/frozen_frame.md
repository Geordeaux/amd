---
id: frozen_frame
title: Frozen Frame
hide_title: true
---

# Frozen Frame 指标

## 背景

Frozen Frame 在 Google 的官方文档上给出的定义如下：

>  Percentage of screen instances during which more than 0.1% of frames took longer than 700 ms to render.

中文翻译过来就是 **对于指定的页面，如果有超过 700ms 的渲染时间占据总停留时间的比例，超过 0.1%** 就被定义为 Frozen Frame 事件。

Frozen Frame 相对于 Slow Rendering 事件，更加暴露了页面的问题。Slow Rendering 在用户感知上，是一个抽样检测。而 Frozen Frame 更是一个主动检测的机制。从而在两个维度上，来反映对应页面的卡顿问题。

## 上报字段

| 属性 | 类型 | 固定值 | 说明 |
| --- | --- | --- | --- |
| t  | MonitorType | FROZEN-FRAME | 类型 |
| ct | long |  |  单页面停留时间（ms） |
| at  | long |  |  Frozen Frame 时间（ms）|
| p | string |  |  上报页面名称 |

## 上报策略

使用 RunLoop 的监听者模式求得一帧的渲染时间。如果一次渲染超过 700ms 则进行打点上报。通过 Frozen 时间与用户停留时间求商，计算出 Frozen Frame 的 Ratio 占比。

## 北极星分数公式

以下变量列表及含义如下：

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f1.png)

计算公式：

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f2.png)



## JSON 点信息

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

## LifeCycleChecker 模块

![](https://static.devfdg.net/static/mono-static/docs-ui/img/image2020-11-17_11-2-33.png)

`LifeCycleChecker` 是每一个 VC 都应该持有的一个对象，这个对象会注入到 VC 中常用的生命周期方法和自定义的符号方法中。

当我们需要完成一个生命周期的监听需求时，我们可以对应的构造一个遵循 `LifeCycleTrace` 协议的 `Trace` ，并实现 `create(sceneName:sceneType:) -> Self?` 方法，从而完成 adaptor 的注入。

## 什么是 Trace ？

下面我们用 First Frame 进行举例，首先我们来创建一个 `FirstFrameLifeCycleTrace` ，并且遵循 `LifeCycleTrace` 协议来实现规定的方法。其中 `LifeCycleTrace` 的定义如下：

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

这里我们可以看出，**Trace 其实是对于一个监控事件的描述，这里定义了开始（start）、触发（active）和结束（finish）三个时机的回调。**

First Frame 其实就是在记录一个 VC 第一次 loadView 到渲染完成的时间 `viewDidAppear`。所以对应的，`FirstFrameLifeCycleTrace` 我们可以如下实现：

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

// 这是一个初始化 Rules 工厂，这里只放出 First Frame 的工厂方法
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

## `LifeCycleTrace` 和 `LifeCycleTraceSession` 的关系

`LifeCycleTraceSession` 是 `LifeCycleTrace` 的载体，每一个 `LifeCycleTraceSession` 都会持有一个 `LifeCycleTrace`。

![](https://static.devfdg.net/static/mono-static/docs-ui/img/image2020-11-17_13-21-30.png)

通过 LifeCycleTrace 的 create 方法，会生成一个对应的 LifeCycleTraceSession 并加入到当前的 LifeCycleChecker 中。然后 VC 触发对应规则中的方法时，会遍历所有的 LifeCycleTraceSession，根据内部封装的规则，从而响应对应 trace 的回调。
