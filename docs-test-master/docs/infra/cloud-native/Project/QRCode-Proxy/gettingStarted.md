---
id: gettingStarted
title: Getting Started
---
# 1.Determine your QR code format
Standard
https://{your domain}/qr/{hash}

Custom
If your QRCodes has been published or your QRCodes has been used for other purposes. You can contact us then we will integrate according to your custom format



# 2.Determine where to jump
QRCode Service support jump to the page under the Binance's domains or Binance mini-program(DeepLink)
the jump page need development by Binance engineer



# 3.How to Pass parameters
Fixed parameters
you can pass fixed parameters to the page, you should supply this parameters when configuring for integrated development

Custom parameters
Custom parameters We can call apis you provided to generate parameters
your api must be
Method: Get
Header: content-type:application/json
Path:https://{your domain}/{your path}?hash=xxxx

Response
```jsx
    {
        "code": "000000",
        "message": null,
        "data": {
          "params": [
            {"key":"key1","value":"value1"},
            {"key":"key2","value":"value2"}
          ]
        }
    }
```
then parameters will behind with deeplink



# 4.Circuit Breaker Config
To make sure every Qrcode run will, you need support us complete this config
EnableFuse: 是否启动熔断


Timeout: 执行的超时时间
how long to wait for command to complete


MaxConcurrentRequests：最大并发量
how many commands of the same type can run at the same time


SleepWindow：当熔断器被打开后，SleepWindow 的时间就是控制过多久后去尝试服务是否可用了
is how long, in milliseconds, to wait after a circuit opens before testing for recovery


RequestVolumeThreshold： 一个统计窗口 10 秒内请求数量。达到这个请求数量后才去判断是否要开启熔断
is the minimum number of requests needed before a circuit can be tripped due to health


ErrorPercentThreshold：错误百分比，请求数量大于等于 RequestVolumeThreshold 并且错误率到达这个百分比后就会启动熔断
causes circuits to open once the rolling measure of errors exceeds this percent of requests


# 5. Start Scan QRCode
After Complete all the config above, user can scan qrcode in Binance App and jump to the Deeplink 
