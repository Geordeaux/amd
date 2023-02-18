---
id: index
title: Standard
---

# 主要指标

## LCP
  LCP (Largest Contentful Paint)是从浏览器(或webview)load页面的url开始，到页面上最大元素渲染出来的时间，单位ms，类型Int，是度量页面的打开速度的主要指标。详见 [LCP](https://web.dev/lcp/)  
- LCP p75  
  - 优秀：<1.5s
  - 良好：1.5s~2.5s
  - 差：2.5s~

## FCP
  FCP (First Contentful Paint) 是从浏览器(或webview)load页面的url开始，到屏幕上呈现页面任意内容的时间。“内容”是指文本，图像（包括背景图像），svg元素或非白色canvas元素。单位ms，类型Int，是度量页面的打开速度的指标之一。详见 [FCP](https://web.dev/fcp/)  
- FCP p75  
  - 优秀：<1s
  - 良好：1s~2s
  - 差：2s~ 

## FID
  FID (First Input Delay)是从用户第一次与页面进行交互（即当他们单击页面上的链接，点击按钮）到浏览器开始处理事件（即运行相关JS）的时间，单位ms，类型Int，是度量页面响应速度的指标。详见 [FID](https://web.dev/fid/)  
- FID p75  
  - 优秀：<100ms
  - 良好：100ms~300ms
  - 差：300ms~

## CLS
  CLS (Cumulative Layout Shift) 是在页面整个打开期间中发生的页面元素位置移动分数的总和，没有单位，类型为Float，是度量页面视觉稳定性的指标。详见 [CLS](https://web.dev/cls/)  
- CLS p75  
  - 优秀：<0.1
  - 良好：0.1~0.25
  - 差：0.25~


## Polaris Score
  Polaris Score(北极星指标)是对页面整体性能的总评分(0～100分)，它是对在用户设备上采集到的页面性能数据进行公式计算和加权计算(FCP 25%/LCP 45%/FID 20%/CLS 10%)而得到的总分
#### 得分曲线  
![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/metric.png)

#### 分数计算
![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/calc.png)
  - 步骤1.分别获取pc端和mobile端，FCP/LCP/CLS/FCP的p75分位值
  - 步骤2.带入getScore函数（[函数源码](https://git.toolsfdg.net/fe/score-add-api/blob/master/src/calc.js)）进行运算，得到pc和mobile的分数，如下图
    ![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/getscore.png)
  - 步骤3.pc和mobile的分数根据pv数据，加权计算，得到最终的分数，即为Polaris Score

#### 评分
  - 100分 - Superstar
  - 90分 - Outstanding Performer
  - 80分 - Exceeding Expectations
  - 70分- Meets Basic Expectations
  - 60分- Below Expectations	


# 其他指标

## UI Freeze Time
  是页面UI卡顿(无响应)的时间，JS[长任务](https://web.dev/long-tasks-devtools/#what-are-long-tasks)会阻塞浏览器主线程，导致UI卡顿，单位ms，类型Int
- 优秀：< 500ms
- 良好：500ms ~ 1.5s
- 差： 1.5s~
## Doc Load Time
  是把doc完整下载下来所花费的时间，由2个因素决定：1.doc大小 2.网络的好坏
- 优秀：< 350ms
- 良好：350ms~650ms
- 差： 650ms～

## Doc Size  
 Doc大小  
- 优秀：< 14KB
- 良好：14KB~50KB
- 差： 50KB～

## Doc DNS/CON/TLS/TTFB
 Doc域名的解析建立连接各阶段时间
- DNS p75  
  - 优秀：< 50ms
  - 良好：50ms~90ms
  - 差： 90ms～
- CON p75  
  - 优秀：< 50ms
  - 良好：50ms~90ms
  - 差： 90ms～
- TLS p75  
  - 优秀：< 50ms
  - 良好：50ms~90ms
  - 差： 90ms～
- TTFB p75  
  - 优秀：< 500ms
  - 良好：500ms~700ms
  - 差： 700ms～

## DOMContentLoaded
  是从浏览器(或webview)load页面的url开始，到HTML文档被完全加载和解析完成的时间，基本上等于doc+js的下载和执行时间
- 优秀：< 850ms
- 良好：850ms~1.2s
- 差： 1.2s～

## Resources Per Request (cpu)
一个服务cpu占用总量/该服务每秒请求数

## Resources Per Request (mem)
一个服务mem占用总量/该服务每秒请求数

## 计算指标1
每秒出站请求数/每秒入站请求数

## 计算指标2
每秒出站请求数p90/每秒入站请求数p90

## API Request Time 
 API请求时长 
p75
- 优秀：< 200ms
- 良好：200ms~400ms
- 差： 400ms～

## API Error Ratio
 API错误数占比
- 优秀：> 99.9%
- 良好：99.9%~97%
- 差： < 97%

