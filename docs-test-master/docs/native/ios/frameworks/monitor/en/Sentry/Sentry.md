---
id: Sentry
title: Sentry
hide_title: true
---

# Sentry

## 一.Format Definition

1. Field Description

   | Key  | Type      | Description                               |
   | ---- | --------- | ---------------------------------- |
   | t    | String    | t=SENTRY                    |
   | u    | URL？     |                                    |
   | c    | Int       | BUCode + ModuleCode + BusinessCode |
   | bc   | String?   | detail error message                     |
   | d    | TimeStamp | log report time                           |

2. Code Definition

  The error code is set to 6 digits, the first digit of the code (BUCode), the second to 3 digits (ModuleCode: error module), the 4th to 6th digits (BusinessCode: error code), where `ModuleCode` and `BusinessCode` are required Each business defines itself.

   | BU Code | Code   | Description |
   | ------- | ------ | ---- |
   | Com     | 100000 |      |
   | Finance | 200000 |      |
   | C2C     | 300000 |      |
   | Fiat    | 400000 |      |
   | ...     |        |      |

3. The complete data format reported to the server

   ```
   {
       "screen-size": "375.0*812.0",
       "data-list": [{
           "d": 1612502597.962816,
           "bc": "xxx",
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

## 二.Monitoring of key business scenarios
1. Code defined for the Margin module under Finance BU

   | Business Name                                 | Code | Description |
   | ---------------------------------------- | ---- | ---- |
   | OrderBook-ws state abnormal      | 10   |      |
   | OrderBook-bids和asks abnormal           | 11   |      |
   | OrderBook-closePrice abnormal            | 12   |      |
   |                                          |      |      |
   | Trade - MarketFeed connect failed              | 21   |      |
   | Trade - margin cross does not support the current symbol | 22   |      |
   | Trade - Failed to create a normal order for cross  | 23   |      |
   | Trade - Failed to create a borrow order for cross               | 24   |      |
   | Trade - Failed to get cross asset details | 25   |      |
   | Trade - Failed to create a repay order for cross               | 26   |      |
   | Trade - Failed to create a normal order for isolated | 33   |      |
   | Trade - Failed to create a borrow order for isolated | 34   |      |
   | Trade - Failed to get isolated asset details | 35   |      |
   | Trade - Failed to create a repay order for isolated | 36   |      |

2. Code rules such as Margin

   ```
   extension MonitorTrackerCode {
       static let marginModule = 220000
   
       /// Trade - MarketFeed connect failed
       static let tradeNoMarket = marginModule + 21
       /// Trade - margin cross does not support the current symbol
       static let tradeNoSymbol = marginModule + 22
       /// Trade - Failed to create a normal order for cross
       static let tradeCrossNormalOrder = marginModule + 23
       /// Trade - Failed to create a borrow order for cross
       static let tradeCrossBorrowOrder = marginModule + 24
       /// Trade - Failed to get cross asset details
       static let tradeCrossBorrowAvailableAsset = marginModule + 25
       /// Trade - Failed to create a repay order for cross
       static let tradeCrossRepayOrder = marginModule + 26
   
       /// Trade - Failed to create a normal order for isolated
       static let tradeIsolatedNormalOrder = marginModule + 33
       /// Trade - Failed to create a borrow order for isolated
       static let tradeIsolatedBorrowOrder = marginModule + 34
       /// Trade - Failed to get isolated asset details
       static let tradeIsolatedBorrowAvailableAsset = marginModule + 35
       /// Trade - Failed to create a repay order for isolated
       static let tradeIsolatedRepayOrder = marginModule + 36
   }
   ```

3. Example

   ```
   Monitor.shared.logMonitor(code: .tradeCrossNormalOrder, message: error.localizedDescription)
   ```
