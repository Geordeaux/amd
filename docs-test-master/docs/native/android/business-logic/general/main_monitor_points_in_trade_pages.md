---
id: main_monitor_points_in_trade_pages
title: Main monitor points in trade pages

hide_title: true
---

# Main monitor points in trade pages

## Tools

- logan
- 神策
- firebase

---

## How to add monitor poins

- In successful and failure cases when fetching data from server.
- Before placing a order (report current margin mode, whether it's active account and related device info).
- Tap placing a order.
- Before invoked **placeorder function.**
- Invoke **placeorder function** successful**.**
- Invoke **placeorder function** failed**.**
- account wss.
- Liquidation/"Take profit or stop loss"
- Confirm the order
- One-click liquidation(future)
- Add target price(options)
- Extension(options)

## User balance

- Refresh wsws
- Refresh asset informations

---

## Monitor Check Points

### General

"$os_version":"10",

"$device_id":"f877b7c12934d93e",

"$model":"HD1910",

"$os":"Android",

"$manufacturer":"OnePlus",

"$app_version":"1.31.0",

"$wifi":true,

"$network_type":"WIFI",

"uid",

"time",//

"statue",//place,success,fail

### spot-trade

"symbol", // Currency pair

"side",    // Buy or sell

"type",           // Order type

"latestPrice"  // Latest Price 

“amount”          // Include the field expect the **opposite market order**

“totalMarketAmount ” // Only use the field when it's the **opposite market order**

"marketType", // Whether it is **opposite market order**

"price",         // Price 

"stopPrice", // Trigger Price

"limitPrice", // Limit price 

"orientation" // Whether it is landscape portrait

### margin-trade

"symbol", // Currency pair

"side", // Buy or sell

"type", // Order type

"marketType", // Whether it is **opposite market order**

"latestPrice", // Latest Price 

"sideEffectType", // borrow mode

“amount”          // Include the field expect the **opposite market order**

“totalMarketAmount ” // Only use the field when it's the **opposite market order**

"price", // Price

"stopPrice", //Trigger Price

"limitPrice", // Limit price

"position", // What's current position

"orientation" // Whether it is landscape portrait

### u-future-trade

"symbol",

"side",

"positionSide",

"leverage",

"type",

"time_in_force",

"reduce_only",

"postOnly",

"strategyType",

"priceProtect",

### coin-future-trade

"type",

"symbol",

"side",

"positionSide",

"leverage",

"type",

"time_in_force",

"reduce_only",

"postOnly"

"time_in_force",

### options-trade

for example：

"symbol",

"direction",

"timeFrame",

"higherDeltaTargetPrice",

"lowerDeltaTargetPrice",

"walletType",

### options-extend

for example：

"symbol",

"optionsId",

"direction",

"timeFrame",

"higherTargetPrice",

"lowerTargetPrice",

"walletType"

### spot-balance

"asset",

"free",

### margin-balance

"asset",

"free",

"borrowed",

"total",

### u-future-balance

"asset",

"walletBalance",//wb

"crossWallet" //cw

### coin-future-balance

"asset",

"walletBalance",//wb

"crossWallet" //cw

---
[tiger.han@binance.com](https://confluence.toolsfdg.net/display/~tiger.han@binance.com) is responsible for it.

It will hava a meeting every friday afternoon.