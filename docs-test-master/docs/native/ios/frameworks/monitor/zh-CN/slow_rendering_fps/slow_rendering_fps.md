---
id: slow_rendering_fps
title: Slow Rendering(FPS)
hide_title: true
---

# FPS 与 Slow Rendering 采集

## 背景

### FPS 指标

FPS 是指图像连续在显示设备上出现的频率。FPS 低，表示 App 不够流畅，还需要进行优化。
但是，和前面对 CPU 使用率和内存使用量的监控不同，iOS 系统中没有一个专门的结构体，用来记录与 FPS 相关的数据。

对 FPS 的监控也可以比较简单的实现：**通过注册 `CADisplayLink` 得到屏幕的同步刷新率，记录每次刷新时间，然后就可以得到 FPS**。

### Slow Rendering 指标

Slow Rendering 在 Google 官方文档中给出的定义如下：

> Percentage of screen instances during which more than 50% of frames took longer than 16 ms to render.

其实就是对应到单一页面的时候，超过 `50%` 帧渲染时间超过 `16ms` 则说明具有 *Slow Rendering* 。而对于 FPS 而言，恰好就是帧渲染时间的倒数。我们经常说 `60Hz` 是一个流畅状态，对应的刷新频率其实是通过以下公式进行计算：

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f1.png)

## 上报字段

| 属性 | 类型 | 固定值 | 说明 |
| --- | --- | --- | --- |
| t  | MonitorType | FPS | 类型 |
| d  | TimeStamp |  |  开始时间 |
| f | int |  |  FPS值(每秒) |
| p | string |  |  上报页面名称 |

## 上报策略

**目前采用轮训方式，每隔 2 分钟进行一次 FPS 采样进行上报**。这个时间阈值可以通过 Apollo 进行动态配置。

## 北极星分数公式

以下是公式中所有变量定义：

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f2.png)

计算公式：

![](https://static.devfdg.net/static/mono-static/docs-ui/img/f3.png)

数据样例链接：https://docs.google.com/spreadsheets/d/1PjBneE0RQFWIXYMOZUzVsOMmlizJmdWMtwFVvPAawdw/edit#gid=0

## JSON 点信息

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

## iOS 端核心代码

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

但值得注意的是，在屏幕中显示图像的过程中，CPU负责计算显示内容，进行诸如视图创建，布局计算，图片解码等工作，然后将数据提交到 GPU 上，而GPU对这些图像数据进行变换，渲染之后，会把图像提交到帧缓冲区，然后在下一次同步信号来临的时候，将图像显示到屏幕上。然后GPU就切换指向到另一个帧缓冲区，重复上述工作。

由此可以得知，因为 `CADisplayLink` 的运行取决于 `RunLoop`。而 `RunLoop` 的运行取决于其所在的 mode 以及 CPU 的繁忙程度，当 CPU 忙于计算显示内容或者 GPU 工作太繁重时，就会导致显示出来的 FPS 与 Instrument 的不一致。

故使用 `CADisplayLink` 并不能很准确反映当前屏幕的FPS！所以此方法需要综合 CPU 的占用率并发筛选才能得到有效且可参考的 FPS 数据。
