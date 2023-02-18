---
id: multi_mainnet_deposit_and_withdrawal
title: Multi-mainnet deposit and withdrawal
hide_title: true
---


# Multi-mainnet deposit and withdrawal

### Contracts ：richard.cao@binance.com

### Doc:

[https://confluence.toolsfdg.net/pages/viewpage.action?pageId=23732363](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=23732363)

### UI:

[https://www.figma.com/file/PBVaq1GU6QGv0zDpDcDIcD/%E5%85%85%E5%80%BC%E6%8F%90%E7%8E%B0%E5%88%87%E6%8D%A2%E4%B8%BB%E7%BD%91?node-id=0%3A1](https://www.figma.com/file/PBVaq1GU6QGv0zDpDcDIcD/%E5%85%85%E5%80%BC%E6%8F%90%E7%8E%B0%E5%88%87%E6%8D%A2%E4%B8%BB%E7%BD%91?node-id=0%3A1)

### Background

1. Only owe deposits and withdrawals: currency transactions. Deposit and withdrawal are related.
2. Entrance for deposit and withdrawal:
    1. Homepage → Funds → Spot tab → Deposit and Withdrawal Button
    2. Homepage → Funds → Spot tab → Select Assets → Deposit and Withdrawal
3. The basic unit of deposit and withdrawal, which used to be the currency, is now the mainnet.
4. For example: before it was based on btc deposit and withdrawal, now it is based on btc → BEP2 or btc → btc. 
The bep2 mainnet and btc mainnet are the two mainnets under the currency btc, which is one more dimension. This dimension belongs to the currency.

Steps

![img](https://confluence.toolsfdg.net/download/attachments/27876958/%E6%9C%AA%E5%91%BD%E5%90%8D%E6%96%87%E4%BB%B6%20%281%29.png?version=1&modificationDate=1581583660000&api=v2)

### Interface:

/gateway-api/v1/private/capital/config/getAll

The interfaces of the multi-main network are all in the CapitalRepository. 

All dimensions are the mainnet under the currency.

### Important description of return value:

`coin->depositallenable /withdrawallenable` 
Whether deposit/withdrawal of this currency is allowed, if it is false, there will be a pop-up prompt, and all mainnets of this currency cannot be deposited or withdrawn.

`coin->networkList`

The main network list under the currency.

`network -> withdrawEnable/depositEnable` 

Whether the mainnet allows deposit/withdrawal or not, if it is false, there will be a red letter prompt, and some mainnets under this currency can be deposited and withdrawn.

`network->withdrawMin` 

withdrawal range is withdrawMin <= withdrawal value <= position

`network -> sameAddress` 

The deposit and withdrawal are one currency. At this time, you need to fill in the tag, and the tag is the remark.

`network -> label` 

When adding an address, label is a label. After the addition is successful, it will be placed at the top of the drop-down box to distinguish the address.

### Related classes

Deposit：DepositActivity

Withdraw：WithdrawActivity