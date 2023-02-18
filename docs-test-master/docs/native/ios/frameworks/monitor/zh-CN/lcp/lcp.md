---
id: lcp
title: LCP
hide_title: true
---

## LCP

### 上报字段

| Property | Type        | Default | Required | Description            |
| -------- | ----------- | ------- | -------- | ---------------------- |
| t        | MonitorType | LCP     | true     |                        |
| ct       | long        |         | true     | 整体耗时，单位ms       |
| p        | string      |         | true     | 上报页面名称           |
| fcp      | long        |         |          | 页面创建耗时 (iOS专有) |
| pt       | long        |         |          | 页面渲染耗时 (iOS专有) |
| reqt     | long        |         |          | 页面请求耗时           |
| ft       | long        |         |          | 页面首帧时间耗时       |

### JSON 数据

```json
{
    "data-list": [{
    "ipaMode": "DEBUG",
		"t": "LCP",
		"ft": 230.24201393127441,
		"wifi": true,
		"pt": 13.299942016601562,
		"d": 1611123936.5801549,
		"fcp": 87.769985198974609,
		"at": 0,
		"ct": 554.85177040100098,
		"ci": "57.7",
		"reqt": 453.7818431854248,
		"p": "BNCFuturesModule.FutureTradeTableViewController"
    }],
    "screen-resolution": "1125.0*2436.0",
    "system-version": "14.0.1",
    "screen-size": "375.0*812.0",
    "operator": "中国移动",
    "jailbreaked": false,
    "ipa-mode": "DEBUG",
    "sessionID": "C1750826-D810-402D-8BFE-4B5C56E8815E",
    "brand": "iPhone Xs",
    "screen-ppi": 458,
    "network": "中国移动4G"
}
```

### 分数计算

Value is the percentile_75 time cost in seconds of largest contentful paint, Score follow[ Scoring Curve](https://docs.google.com/document/d/1evdX-oOSL_Sg1Zan8yTXn8Z7_O_6RJEFF_HLk0atzd0/edit#heading=h.1rogx99ve37v)

![Xnip2021-01-18_15-00-02](https://static.devfdg.net/static/mono-static/docs-ui/img/Xnip2021-01-18_15-00-02.jpg)

| Value | 0.3  | 1.2  | 1.8  | 2.4  | 3.1  | 4.0  | 5.0  | 6.5  |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Score | 100  | 90   | 80   | 70   | 60   | 50   | 40   | 30   |

### 背景

![img](https://web.dev/vitals/lcp_8x2.svg)

Largest contentful paint（LCP）用于衡量页面上最重要内容（或者可视区域中最大元素）的渲染时间，落地到客户端可以转换成页面的加载时间。

简单描述下 Web 页面上计算 LCP 的方式：

Web 页面在加载过程中，是线性的，元素是一个一个渲染到屏幕上的，每当有一个新的元素渲染完就会记录一个 PerformanceEntry 对象，所以“渲染面积”最大的元素随时在发生变化。

该过程将持续到用户第一次滚动页面或第一次用户输入（鼠标点击，键盘按键等），也就是说，一旦用户与页面开始产生交互，LCP 即视为结束。

落地到客户端则转换成页面的加载时长，如下图所示：

![img](https://awps-assets.meituan.net/mit-x/blog-images-bundle-2016/d8f483a5.png)

其中 T1 指页面初始化到第一个 UI 元素显示的时间，这个 UI 元素一般是指数据加载时的等待动画之类的。T2 是指网络请求时间，这个时间的开始点有可能早于 T1 的结束点。T3 是加载到数据后，为 UI 填充数据并重新渲染完成的时间。T 是整个页面从初始化到最终 UI 绘制完成的时间。

### 方案

依赖 `LifeCycleChecker` 模块，获取页面各个时机点。开始点为页面创建时 `loadView`，结束点为 `refreshView` 且 `state == .success`。

`initTime` 页面的创建时间：`viewDidLoad` - `loadView`

`renderTime` 页面渲染时间：`viewDidLayoutSubviews` - `viewWillLayoutSubviews`

`requestTime` 页面请求时间：`refreshView` - `viewDidLoad`

目前 `LifeCycleChecker` 所有时机均在 `BaseViewController` 里面打点，如果子类在该方法里做了耗时操作，这时统计到的时间是并不一定准确的。因此在计算各个时间的结束点时，会获取下一个 `runloop` 的时间作为结束点。

### 核心代码

```
func activeActionHandler(occasion: LifeCycleTraceOccasion, state _: LifeCycleTraceState) {
    let currentLoopTime = Date().timeIntervalSince1970
    DispatchQueue.main.async {
        let nextLoopTime = Date().timeIntervalSince1970
        self.recordActiveState(occasion, currentLoopTime: currentLoopTime, nextLoopTime: nextLoopTime)
    }
}

func recordActiveState(_ occasion: LifeCycleTraceOccasion, currentLoopTime: TimeInterval, nextLoopTime: TimeInterval) {
    switch occasion {
    case .viewDidLoad:
        initTime = currentLoopTime - startTime
        requestStartTime = currentLoopTime
    case .viewWillAppear where firstWillAppearTime == 0:
        firstWillAppearTime = currentLoopTime
    case .viewDidAppear where firstDidAppearTime == 0:
        firstDidAppearTime = nextLoopTime
    case .viewWillLayoutSubviews:
        lastViewWillLayoutTime = currentLoopTime
    case .viewDidLayoutSubviews where lastViewWillLayoutTime > 0 && requestStartTime == 0:
        renderTime += (currentLoopTime - lastViewWillLayoutTime)
    case .refreshViewState where requestStartTime > 0 && requestTime == 0:
        requestTime = currentLoopTime - requestStartTime
    case let .custom(selName):
        switch selName {
        case LifeCycleTraceOccasion.requestBegin where requestStartTime > 0:
            requestStartTime = currentLoopTime
        case LifeCycleTraceOccasion.requestEnd where requestStartTime > 0 && requestTime == 0:
            requestTime = currentLoopTime - requestStartTime
        default:
            break
        }
    default:
        break
    }
    log(occasion, currentLoopTime: currentLoopTime, nextLoopTime: nextLoopTime)
}

func finishActionHandler(occasion: LifeCycleTraceOccasion, state _: LifeCycleTraceState) {
    let currentTime = Date().timeIntervalSince1970
    if requestTime == 0, requestStartTime > 0 {
        requestTime = currentTime - requestStartTime
    }
    DispatchQueue.main.async {
        let nextLoopTime = Date().timeIntervalSince1970
        self.reportIfNeeded(
            initTime: self.initTime,
            renderTime: self.renderTime + (nextLoopTime - currentTime),
            requestTime: self.requestTime,
            firstFrameTime: max(self.firstDidAppearTime - self.startTime, 0),
            totalTime: nextLoopTime - self.startTime,
            pageName: self.pageName
        )
        self.log(occasion, currentLoopTime: currentTime, nextLoopTime: nextLoopTime)
    }
}
```

### 参考链接

[移动端性能监控方案Hertz](https://tech.meituan.com/2016/12/19/hertz.html#页面加载时间)

[Binance Web Polaris Performance Score](https://docs.google.com/document/d/1evdX-oOSL_Sg1Zan8yTXn8Z7_O_6RJEFF_HLk0atzd0/edit#heading=h.3i77smbzi7b0)

[Lighthouse performance scoring](https://web.dev/performance-scoring/)

[Largest Contentful Paint (LCP)](https://web.dev/lcp/)
