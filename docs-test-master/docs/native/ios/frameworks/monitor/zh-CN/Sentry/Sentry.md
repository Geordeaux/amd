---
id: Sentry
title: Sentry
hide_title: true
---

# Sentry

## 一.格式定义

1. 上报字段

   | Key  | 类型      | 说明                               |
   | ---- | --------- | ---------------------------------- |
   | t    | String    | 固定取值 SENTRY                    |
   | u    | URL？     |                                    |
   | c    | Int       | BUCode + ModuleCode + BusinessCode |
   | bc   | String?   | 详细的报错信息                     |
   | d    | TimeStamp | 开始时间                           |

2. Code定义

   错误码定为6位数，code第一位（BUCode）,第2位-3位（ModuleCode：错误模块），第4-6位（BusinessCode：错误代码）,其中`ModuleCode`和 `BusinessCode`需要各个业务自行定义。

   | BU Code | Code   | 描述 |
   | ------- | ------ | ---- |
   | Com     | 100000 |      |
   | Finance | 200000 |      |
   | C2C     | 300000 |      |
   | Fiat    | 400000 |      |
   | ...     |        |      |

3. 上报JSON 格式

   ```
   {
       "screen-size": "375.0*812.0",
       "data-list": [{
           "d": 1612502597.962816,
           "bc": "最大借款额度获取失败",
           "t": "SENTRY",
           "c": 220025
       }],
       "network": "中国移动3G",
       "brand": "iPhone Xs",
       "system-version": "14.4",
       "screen-ppi": 458,
       "operator": "中国移动",
       "ipa-mode": "DEBUG",
       "jailbreaked": false,
       "screen-resolution": "1125.0*2436.0",
       "sessionID": "B0AA6328-0281-4100-8D31-C8E449EADDF7"
   }
   
   ```

## 二.关键业务场景的监控
1. 针对Finance BU下的Margin 模块定义的Code

   | 业务名称                                 | Code | 描述 |
   | ---------------------------------------- | ---- | ---- |
   | OrderBook-ws状态异常                     | 10   |      |
   | OrderBook-bids和asks数据异常             | 11   |      |
   | OrderBook-closePrice 数据异常            | 12   |      |
   |                                          |      |      |
   | Trade - MarketFeed 连接失败              | 21   |      |
   | Trade - 全仓不支持当前symbol             | 22   |      |
   | Trade - 创建全仓普通单失败               | 23   |      |
   | Trade - 创建全仓借款单失败               | 24   |      |
   | Trade - 创建全仓借款单前获取资产信息失败 | 25   |      |
   | Trade - 创建全仓还款单失败               | 26   |      |
   | Trade - 创建逐仓普通单失败               | 33   |      |
   | Trade - 创建逐仓借款单失败               | 34   |      |
   | Trade - 创建逐仓借款单前获取资产信息失败 | 35   |      |
   | Trade - 创建逐仓还款单失败               | 36   |      |

2. code使用规则 如Margin

   ```
   extension MonitorTrackerCode {
       static let marginModule = 220000
   
       /// Trade - MarketFeed 连接失败
       static let tradeNoMarket = marginModule + 21
       /// Trade - 全仓不支持当前symbol
       static let tradeNoSymbol = marginModule + 22
       /// Trade - 创建全仓普通单失败
       static let tradeCrossNormalOrder = marginModule + 23
       /// Trade - 创建全仓借款单失败
       static let tradeCrossBorrowOrder = marginModule + 24
       /// Trade - 创建全仓借款单前获取资产信息失败
       static let tradeCrossBorrowAvailableAsset = marginModule + 25
       /// Trade - 创建全仓还款单失败
       static let tradeCrossRepayOrder = marginModule + 26
   
       /// Trade - 创建逐仓普通单失败
       static let tradeIsolatedNormalOrder = marginModule + 33
       /// Trade - 创建逐仓借款单失败
       static let tradeIsolatedBorrowOrder = marginModule + 34
       /// Trade - 创建逐仓借款单前获取资产信息失败
       static let tradeIsolatedBorrowAvailableAsset = marginModule + 35
       /// Trade - 创建逐仓还款单失败
       static let tradeIsolatedRepayOrder = marginModule + 36
   }
   ```

3. 具体使用姿势

   ```
   Monitor.shared.logMonitor(code: .tradeCrossNormalOrder, message: error.localizedDescription)
   ```
