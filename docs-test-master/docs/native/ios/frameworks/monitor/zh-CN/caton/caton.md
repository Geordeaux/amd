---
id: caton
title: Ping 主线程卡顿
hide_title: true
---

## 业界四种方案情况概述

|   | FPS | Ping | Runloop | Hook msgSend |
| --- | --- | --- | --- | --- |
| 卡顿反馈 | 高，可以及时反应用户感知状态，但是会采集较多无用信息 | 高，可以有效收集主线程卡顿，并且可以控制卡顿上限阈值 | 中高，会有某些事件无法反馈。例如 GCD 和 OperationQueue | 很高，也会带来相当大的函数调用开销 |
| 采集精度 | 低，CPU 空闲时回调，栈信息采集不精确 | 中高，可以及时获取栈信息，但是不排除误报 | 中高，可以及时获得栈信息 | 很高，因为纬度是方法级别，不局限于线程级别 |
| 性能开销 | 中低，唤醒 runloop 处理 | 中，要一个常驻子线程 | 低，仅增加 runloop observer 即可 | 高，要处理全部方法 |
| 开发成本 | 低，业界很多实现方案，主要趋于 CADisplayLink | 低，只需要上层封装即可 | 中低，实现代码量较大。但成熟方案很多 | 高，需要内联汇编，而且 x86 和 ARM 要两版开发，目前网上已有 ARM 成熟方案 |
| 各厂使用情况 | 头条 Slardar、网易云捕、阿里（b 系） | 微信 Matrix 内部版、百度 | 头条 Slardar、微信 Matrix、腾讯 Bugly、网易云捕、阿里 | 滴滴、阿里 |


**综合上述情况，暂时选定 FPS 和 Ping 主线程方式双打来验证我们的指标准确性。**

## 主线程 Ping 方案的一些设计和思路

![](https://static.devfdg.net/static/mono-static/docs-ui/img/frame.png)

## 核心代码

```swift
private final class CantonPingMainThread: Thread {
     
    private let mainThreadQueue = DispatchQueue.init(label: "APMMonitor.CantonPingMainThread")
     
    private let cap: CGFloat
    private let catchHandler: () -> Void
    private let semaphore = DispatchSemaphore(value: 0)
     
 
    private let lock = NSObject()
    private var _isResponse = true
    private var isResponse: Bool {
        get {
            var ret: Bool = false
            mainThreadQueue.sync {
                ret = _isResponse
            }
            return ret
        }
         
        set {
            mainThreadQueue.async(flags: .barrier) {
                self._isResponse = newValue
            }
        }
    }
     
    init(_ cap: CGFloat, _ handler: @escaping () -> Void) {
        self.cap = cap
        self.catchHandler = handler
        super.init()
    }
     
    override func main() {
        while !isCancelled {
            autoreleasepool {
                isResponse = false
                DispatchQueue.main.async {
                    self.isResponse = true
                    self.semaphore.signal()
                }
                 
                // 停留阈值时间后，检查标记
                Thread.sleep(forTimeInterval: TimeInterval(cap))
                if !isResponse {
                    catchHandler()
                }
                 
                _ = semaphore.wait(timeout: DispatchTime.distantFuture)
            }
        }
    }
}
```

## 上报数据

| 属性 | 类型 | 固定值 | 说明 |
| --- | --- | --- | --- |
| t  | MonitorType | CPU | 类型 |
| d  | TimeStamp |  |  开始时间 |
| si | int |  |  主线程堆栈信息 |
| ci | float |  |  当前 CPU 信息 |
| p | string |  |  上报页面名称 |
| ci | float |  |  CPU 使用量 |
