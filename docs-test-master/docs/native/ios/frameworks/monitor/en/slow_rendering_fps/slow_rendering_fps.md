---
id: slow_rendering_fps
title: Slow Rendering(FPS)
hide_title: true
---

# FPS and Slow Rendering capture

## Background

### FPS 

FPS refers to the frequency at which images continuously appear on the display device. Low FPS means that the App is not smooth enough and needs to be optimized.
However, unlike the previous monitoring of CPU usage and memory usage, there is no special structure in the iOS system to record data related to FPS.

The monitoring of FPS can also be implemented relatively simply: **Get the synchronization refresh rate of the screen by registering `CADisplayLink`, record each refresh time, and then get FPS**.

### Slow Rendering Introduction

*Slow Rendering* is defined in the official Google documents as follows：

> Percentage of screen instances during which more than 50% of frames took longer than 16 ms to render.

When it corresponds to a single page, if the frame rendering time exceeds `50%` and exceeds `16ms`, it means *Slow Rendering*. For FPS, it happens to be the inverse of the frame rendering time. We often say that `60Hz` is a smooth state, and the corresponding refresh rate is actually calculated by the following formula:

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f1.png)

## Field description

| Property | Type | Const Value | Note |
| --- | --- | --- | --- |
| t  | MonitorType | FPS | Tracker Type |
| d  | TimeStamp |  | Tracker Collection Time |
| f | int |  |  FPS per second |
| p | string |  |  ViewController Name |

## Strategy

**Currently, a rotation training method is adopted, and FPS sampling is performed every 2 minutes for reporting**. This time threshold can be dynamically configured through Apollo.


## Polaris Score Calculator

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f2.png)

Data Sample：https://docs.google.com/spreadsheets/d/1PjBneE0RQFWIXYMOZUzVsOMmlizJmdWMtwFVvPAawdw/edit#gid=0

## JSON Introduction

```json
{
  "_index": "events-track-2021-01-21",
  "_type": "doc",
  "_id": "49614671390932215967919561932082058000027633223081134642.0",
  "_version": 1,
  "_score": null,
  "_source": {
    "d": 1611293804123,
    "f": 60,
    "p": "DetailTradeFragment",
    "t": "FPS",
    "bnc-uuid": "7ec30918ec180c6c1fdd92e07b445aeb",
    "versionCode": "1.34.1",
    "clienttype": "android",
    "dt": "2021-01-22 05:36:44.123",
    "system_timestamp": 1611208819799,
    "request_ip": " 130.176.131.70",
    "request_ip0": "176.59.74.232",
    "request_country": "Russian Federation",
    "request_city": "Krasnodar",
    "system_dt": "2021-01-21 06:00:19.799",
    "network": null,
    "system-version": "29",
    "brand": "HONOR|DNN-LX9",
    "log_type": "api"
  },
  "fields": {
    "dt": [
      "2021-01-22T05:36:44.123Z"
    ],
    "system_dt": [
      "2021-01-21T06:00:19.799Z"
    ]
  },
  "highlight": {
    "t": [
      "@kibana-highlighted-field@FPS@/kibana-highlighted-field@"
    ]
  },
  "sort": [
    1611293804123
  ]
}
```

## iOS Core Code

```swift
public func start(_ callback: @escaping (Double) -> Void = { _ in }) {
    self.pingHandler = callback
 
    monitoringTimer = DispatchSource.makeTimerSource(flags: [], queue: DispatchQueue.global())
    displayLink = CADisplayLink(target: self, selector: #selector(displayLinkStep))
    displayLink.add(to: RunLoop.main, forMode: .common)
 
    guard let monitoringTimer = monitoringTimer else {
        return
    }
 
    if isRunning {
        monitoringTimer.suspend()
        self.fpsCounter = 0
        self.displayLink.isPaused = true
    }
 
    monitoringTimer.schedule(deadline: .now(), repeating: Constants.monitoringTimerRepeating)
 
    monitoringTimer.setEventHandler { [weak self] in
        guard let self = self else { return }
        let fps = Double(self.fpsCounter) / Constants.monitoringTimerRepeating
        if fps <= 5 {
            return
        }
        FPSMonitior.lastFps = fps
        self.pingHandler(fps)
        self.fpsCounter = 0
    }
    self.displayLink.isPaused = false
    monitoringTimer.resume()
    isRunning = true
}
```

But it is worth noting that in the process of displaying images on the screen, the CPU is responsible for calculating the display content, performing tasks such as view creation, layout calculation, image decoding, etc., and then submitting the data to the GPU, and the GPU transforms the image data After rendering, the image will be submitted to the frame buffer, and then the image will be displayed on the screen when the next synchronization signal comes. Then the GPU switches to another frame buffer and repeats the above work.

It can be seen that the operation of `CADisplayLink` depends on `RunLoop`. The operation of `RunLoop` depends on the mode it is in and the busyness of the CPU. When the CPU is busy calculating the display content or the GPU is too heavy, it will cause the displayed FPS to be inconsistent with the Instrument.

Therefore, the use of `CADisplayLink` does not accurately reflect the FPS of the current screen! Therefore, this method requires concurrent filtering of the overall CPU occupancy rate to obtain valid and referenceable FPS data.
