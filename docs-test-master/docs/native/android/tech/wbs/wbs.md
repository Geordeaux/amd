---
id: wbs
title: Android-WBS
hide_title: true
---
# Android-WBS

WBS:

1.BU：.com,finance,c2c,fiat,arch

2.Level 1 : mainly responsible for all the large modules, such as homepage, Settings, spot and so on

3.Level 2:  can be thought of as a secondary division of large modules, with granularity depending on whether the business can be split again

4.Dependencies: mainly describes which tripartite and dichotomous businesses the business depends on. Especially the core implementation, such as what KYC depends on, what RISK depends on, etc

5.Owner: the owner of the module

6.CR Owner: the reviewer:


| BU                              | MainMenu             | SecondaryMenu | Dependencies                  | Owner                     | CR owner                  |
| :------------------------------ | :------------------- | :------------ | :---------------------------- | :------------------------ | :------------------------ |
| .com                            | homepage             | lite homepage |                               | klay                      | @klay-wei @fe/android-com |
| pro homepage                    |                      | klay          | @klay-wei @fe/android-com     |                           |                           |
| account                         | account              |               | louzhen                       | @zhen-lou @klay-wei       |                           |
| settings                        |                      | louzhen       | @zhen-lou @klay-wei           |                           |                           |
| share                           |                      | klay          | @klay-wei @fe/android-com     |                           |                           |
| security                        |                      | jerry         |                               |                           |                           |
| withdraw address                |                      | klay          | @klay-wei @hao-qian           |                           |                           |
| reward center                   |                      | klay          | @klay-wei @yunpeng-wang       |                           |                           |
| red packet                      |                      | yisong        | @song-yi @zhen-lou            |                           |                           |
| message                         |                      | klay          | @klay-wei @yunpeng-wang       |                           |                           |
| user                            | login/logout         | jerry         | jerry                         | `@yunpeng-wang @he-wang`  |                           |
| kyc                             | jumio,face++         | zhifeng       | `@zhifeng-li @fe/android-com` |                           |                           |
| 2fa(phone,email, google auther) | google authenticator | klay          | @klay-wei @fe/android-com     |                           |                           |
| forget password                 |                      | qianhao       | @hao-qian @fe/android-com     |                           |                           |
| register                        |                      | yisong        | `@song-yi @yunpeng-wang`      |                           |                           |
| reset 2fa                       |                      | zhifeng       |                               |                           |                           |
| market                          | spot                 |               | jerry                         |                           |                           |
| seach                           |                      | qianhao       | @hao-qian @fe/android-com     |                           |                           |
| future                          |                      |               |                               |                           |                           |
| zone                            |                      |               |                               |                           |                           |
| favorite                        |                      |               |                               |                           |                           |
| wallet                          | spot                 |               | klay                          | @klay-wei @fe/android-com |                           |
| pool                            |                      | qianhao       | `@hao-qian @fe/android-com`   |                           |                           |
| pool transfer                   |                      | wanghe        |                               |                           |                           |
| deposit                         |                      | qianhao       | @hao-qian @klay-wei           |                           |                           |
| withdraw                        |                      | qianhao       | @hao-qian @klay-wei           |                           |                           |
| push                            | push                 | jpush         | jerry                         |                           |                           |
| deeplink                        | deeplink             |               | jerry                         |                           |                           |
| upgrade                         | upgrade              |               | yisong                        | `@song-yi @klay-wei`      |                           |
| webview                         | webview              |               | ivan                          |                           |                           |
| lite version                    | homepage             |               |                               |                           |                           |
| market                          |                      |               |                               |                           |                           |
| wallet                          |                      |               |                               |                           |                           |
| sell/Buy                        |                      |               |                               |                           |                           |
| CoinDetail                      |                      |               |                               |                           |                           |
| Pay Channel                     |                      |               |                               |                           |                           |
| WithDraw                        |                      |               |                               |                           |                           |
| Deposit                         |                      |               |                               |                           |                           |
| Convert                         |                      |               |                               |                           |                           |
| History                         |                      |               |                               |                           |                           |
| biannce card                    |                      |               |                               |                           |                           |
| google pay                      |                      |               |                               |                           |                           |
| Risk                            |                      |               |                               |                           |                           |
| Convert                         |                      |               |                               |                           |                           |
| SensorData                      |                      |               |                               |                           |                           |
| Finance                         |                      |               |                               |                           |                           |
| C2C                             | homepage             | c2c_home      |                               | wengao                    |                           |
| professionAds                   |                      | wengao        |                               |                           |                           |
| ads                             | add_ads              |               | shaoshuai                     |                           |                           |
| edit_ads                        |                      | shaoshuai     |                               |                           |                           |
| order                           | order_detail         |               | guanyang                      |                           |                           |
| make_order                      |                      | wengao        |                               |                           |                           |
| payment                         | payment              |               | guanyang                      |                           |                           |
| make_order_payment              |                      | wengao        |                               |                           |                           |
| Fiat                            |                      |               |                               |                           |                           |
| Arch                            |                      |               |                               |                           |                           |