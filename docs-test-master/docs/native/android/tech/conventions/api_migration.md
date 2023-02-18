---
id: api_migration
title: API Migration
hide_title: true
---

# API Migration

## Contacts

sam.wang@binance.com

|Num.|Desc                             |Old API                                       |New API                                                                     |Dev|Dev test|QA |Release|
|----|---------------------------------|----------------------------------------------|----------------------------------------------------------------------------|---|--------|---|-------|
|    |1.5.4                            |                                              |                                                                            |   |        |   |       |
|1   |Get all products                 |/exchange/public/product                      |/exchange-api/v1/public/asset-service/product/get-products                  |√  |√       |√  |√      |
|2   |Get all asset                    |/assetWithdraw/getAllAsset.html               |/gateway-api/v1/public/asset/asset/get-all-asset                            |√  |√       |√  |√      |
|3   |Symbol currencies                |--                                            |/info-api/v1/public/symbol/currencies                                       |√  |√       |√  |√      |
|4   |Product currency                 |/exchange/public/currency                     |/exchange-api/v1/public/asset-service/product/currency                      |√  |√       |√  |√      |
|5   |Use device                       |--                                            |/gateway-api/v1/private/account/user-device/list                            |√  |√       |√  |√      |
|6   |Get wss url info                 |--                                            |/exchange-api/v1/public/mbxgateway/info/get-wssurl                          |√  |√       |√  |√      |
|7   |Get klin url info                |/exchange/public/klineUrl                     |/exchange-api/v1/public/mbxgateway/info/get-klineurl                        |√  |√       |√  |√      |
|8   |Get depth url info               |/exchange/public/depthUrl                     |/exchange-api/v1/public/mbxgateway/info/get-depthurl                        |√  |√       |√  |√      |
|9   |The token is available           |/user/verifyToken.html                        |/gateway-api/v1/public/authcenter/auth                                      |√  |√       |√  |√      |
|10  |the list of user device          |--                                            |/gateway-api/v1/private/account/user-device/list                            |√  |√       |√  |√      |
|11  |the list of deleted device       |--                                            |/gateway-api/v1/private/account/user-device/delete                          |√  |√       |√  |√      |
|12  |the log of devices               |--                                            |/gateway-api/v1/private/account/user-device/log                             |√  |√       |√  |√      |
|13  |user's dividend asset            |--                                            |/gateway-api/v1/private/asset/asset/user-asset-dividend                     |√  |√       |√  |√      |
|14  |forbidden users                  |--                                            |/gateway-api/v1/private/account/user/forbidden-by-oneself                   |√  |√       |√  |√      |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |1.5.6.0                          |                                              |                                                                            |   |        |   |       |
|15  |Get withdraw address list        |                                              |/gateway-api/v1/private/asset/withdraw-address/get-withdraw-address-list    |√  |√       |√  |√      |
|16  |Save withdraw address            |                                              |/gateway-api/v1/private/asset/withdraw-address/save-withdraw-address        |√  |√       |√  |√      |
|17  |Update white status              |                                              |/gateway-api/v1/private/asset/withdraw-address/update-white-status          |√  |√       |√  |√      |
|18  |Delete white withdraw address    |                                              |/gateway-api/v1/private/asset/withdraw-address/delete-white-withdraw-address|√  |√       |√  |√      |
|19  |Delete withdraw address          |                                              |/gateway-api/v1/private/asset/withdraw-address/delete-withdraw-address      |√  |√       |√  |√      |
|20  |Open withdraw white status       |                                              |/gateway-api/v1/private/account/user/open-withdraw-white-status             |√  |√       |√  |√      |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |1.5.6.1                          |                                              |                                                                            |   |        |   |       |
|21  |User info                        |/user/basedetail.html                         |/gateway-api/v1/private/account/user/base-detail                            |√  |√       |√  |√      |
|22  |Agent info                       |/user/getAgentInfo.html                       |/gateway-api/v1/public/account/agent-info/get                               |√  |√       |√  |√      |
|23  |Pay transition fee by BNB        |/user/setCommissionStatus.html                |/gateway-api/v1/private/account/user/set-commission-status                  |√  |√       |√  |√      |
|24  |Trade oders                      |/exchange/private/tradeOrders                 |/exchange-api/v1/private/streamer/order/get-trade-orders                    |√  |√       |√  |√      |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |1.5.9.0                          |                                              |                                                                            |   |        |   |       |
|25  |Place order                      |/exchange/private/order                       |/exchange-api/v1/private/mbxgateway/order/place                             |√  |√       |√  |√      |
|26  |Get user asset                   |/exchange/private/userAsset                   |/exchange-api/v1/private/asset-service/asset/get-user-asset                 |√  |√       |√  |√      |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |1.6.0~1.6.1                      |                                              |                                                                            |   |        |   |       |
|27  |Gain banner                      |/reInfo/get.html                              |/gateway-api/v1/public/market/all                                           |√  |√       |√  |√      |
|28  |Get announcement                 |/public/getNotice.html                        |/gateway-api/v1/public/market/all                                           |√  |√       |√  |√      |
|29  |Get recommandations              |/asset/recommend.html                         |/gateway-api/v1/public/market/all                                           |√  |√       |√  |√      |
|30  |Delete order                     |/exchange/private/deleteOrder                 |/exchange-api/v1/private/mbxgateway/order/del                               |√  |√       |√  |√      |
|31  |Delete all orders                |/exchange/private/deleteAll                   |/exchange-api/v1/private/mbxgateway/order/delall                            |√  |√       |√  |√      |
|32  |Current open order               |/exchange/private/openOrders                  |/exchange-api/v1/private/streamer/order/get-open-orders                     |√  |√       |√  |√      |
|33  |trade order detail               |/exchange/private/userTradeOrderDetail        |/exchange-api/v1/private/streamer/trade/get-user-trade-detail               |√  |√       |√  |√      |
|34  |Alert price                      |                                              |/gateway-api/v1/private/notification/alertprice/load                        |√  |√       |√  |√      |
|35  |Add alert price                  |                                              |/gateway-api/v1/private/notification/alertprice/add                         |√  |√       |√  |√      |
|36  |Delete alert price               |                                              |/gateway-api/v1/private/notification/alertprice/delete                      |√  |√       |√  |√      |
|37  |Get user's wss url               |/exchange/public/userUrl                      |/exchange-api/v1/public/mbxgateway/info/get-userurl                         |√  |√       |√  |√      |
|38  |Generate user stream             |/exchange/private/startStream                 |/exchange-api/v1/private/mbxgateway/user-stream/start                       |√  |√       |√  |√      |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |1.6.2                            |                                              |                                                                            |   |        |   |       |
|39  |To get User portfolio            |/exchange/private/portfolios                  |/exchange-api/v1/private/asset-service/portfolio/get                        |√  |√       |√  |√      |
|40  |To Add to user portfolio         |/exchange/private/addPortfolio                |/exchange-api/v1/private/asset-service/portfolio/add                        |√  |√       |√  |√      |
|41  |To delete a item in portfolio    |/exchange/private/deletePortfolio             |/exchange-api/v1/private/asset-service/portfolio/del                        |√  |√       |√  |√      |
|42  |To updte portfolio sequence      |/exchange/private/updatePortfolioSequence     |/exchange-api/v1/private/asset-service/portfolio/update-sequence            |√  |√       |√  |√      |
|43  |To fetch server time             |/exchange/public/serverTime                   |/exchange-api/v1/public/mbxgateway/info/get-servertime                      |√  |√       |√  |√      |
|44  |To add dribble btc               |/exchange/private/userAssetTransferDribbletBtc|/exchange-api/v1/private/asset-service/asset/dribblet-btc                   |√  |√       |√  |√      |
|45  |To dribblet asset to bnb         |/exchange/private/dribbletAssetToBNB          |/exchange-api/v1/private/asset-service/asset/dribblet-bnb                   |√  |√       |√  |√      |
|46  |The log of dribblet asset        |/user/userAssetDribbletLog.html               |/exchange-api/v1/private/asset-service/asset/dribblet-log                   |√  |√       |√  |√      |
|47  |login                            |/user/login.html                              |/gateway-api/v1/public/authcenter/login                                     |√  |√       |√  |√      |
|48  |Resend active code               |/user/terminalCodeResend.html                 |/gateway-api/v1/public/account/app/resend-active-code                       |√  |√       |√  |√      |
|49  |To register                      |/user/register.html                           |/gateway-api/v1/public/account/user/register                                |√  |√       |√  |√      |
|50  |Forget pwd                       |/user/terminalForgetPassword.html             |/gateway-api/v1/public/account/app/forget-password                          |√  |√       |√  |√      |
|51  |To get gt-code                   |/security/getGtCode.html                      |/gateway-api/v1/public/common/security/gt-code                              |√  |√       |√  |√      |
|52  |To logout                        |/user/loginOut.html                           |/gateway-api/v1/private/authcenter/logout                                   |√  |√       |√  |√      |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |1.8.0                            |                                              |                                                                            |   |        |   |       |
|53  |Send mobile sms to verify        |/user/sendMobileVerifyCode.html               |/gateway-api/v1/protect/account/mobile/send-mobile-verify-code              |√  |√       |√  |√      |
|54  |Send Binding Mobile a verify code|/user/sendBindMobileVerifyCode.html           |/gateway-api/v1/private/account/user/send-bind-mobile-verify-code           |√  |√       |√  |√      |
|55  |Bind Mobile                      |/user/bindMobile.html                         |/gateway-api/v1/private/account/user/bind-mobile                            |√  |√       |√  |√      |
|56  |Unbind mobile                    |/user/unbindMobile.html                       |/gateway-api/v1/private/account/user/unbind-mobile                          |√  |√       |√  |√      |
|57  |Get country list                 |/country/getList.html                         |/gateway-api/v1/public/country/list                                         |√  |√       |√  |√      |
|58  |generate secret key              |/user/generateSecretKey.html                  |/gateway-api/v1/private/account/user/generate-secret-key                    |√  |√       |√  |√      |
|59  |Bind google verify               |/user/bindGoogleVerify.html                   |/gateway-api/v1/private/account/user/bind-google-verify                     |√  |√       |√  |√      |
|60  |Unbind google verify             |/user/unbindGoogleVerify.html                 |/gateway-api/v1/private/account/user/unbind-google-verify                   |√  |√       |√  |√      |
|61  |Resend pwd code                  |/user/terminalResendPassCode.html             |/gateway-api/v1/public/account/app/resend-pwd-code                          |√  |√       |√  |√      |
|62  |Active certification             |/user/terminalActivate.html                   |/gateway-api/v1/public/account/app/active                                   |√  |√       |√  |√      |
|63  |Verify password                  |/user/terminalPasswordVerify.html             |/gateway-api/v1/public/account/app/password-verify                          |√  |√       |√  |√      |
|64  |reset password                   |/user/terminalResetPassword.html              |/gateway-api/v1/public/account/app/reset-password                           |√  |√       |√  |√      |
|65  |Update password                  |/user/updatePassword.html                     |/gateway-api/v1/private/account/user/update-password                        |√  |√       |√  |√      |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |To fetch K-Line                  |/api/v1/klines                                |                                                                            |   |        |   |       |
|    |To fetch depth                   |/api/v1/depth                                 |                                                                            |   |        |   |       |
|    |To fetch aggTrades               |/api/v1/aggTrades                             |                                                                            |   |        |   |       |
|    |To fetch ticker price            |/api/v1/ticker/price                          |                                                                            |   |        |   |       |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |Configuration                    |                                              |                                                                            |   |        |   |       |
|    |To get year report               |/user/getYearReport.html                      |                                                                            |   |        |   |       |
|    |To get android version           |/telephone/getAndroidVersion.html             |/gateway-api/v1/public/common/config/android-version                        |√  |√       |√  |√      |
|    |To get ref switch                |/getRef.html                                  |/gateway-api/v1/public/common/config/get-ref-switch                         |√  |√       |   |√      |
|    |To get year report status        |/user/getYearReportStatus.html                |/gateway-api/v1/public/common/config/year-report-status                     |√  |√       |   |√      |
|    |To get gas rate                  |/config/getByName.html?name=gas_rate          |                                                                            |√  |√       |   |√      |
|    |To get open lang                 |/user/getopenlang.html                        |                                                                            |√  |√       |   |√      |
|    |To get opening chat time         |/config/appChatOpenSwitch.html                |/gateway-api/v1/public/common/config/appchat-open-status                    |√  |√       |   |√      |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |Money's log                      |                                              |                                                                            |   |        |   |       |
|    |The log of withdraw              |/user/getMoneyLog.html                        |/gateway-api/v1/private/capital/withdraw/list                               |√  |√       |√  |√      |
|    |The log of deposit               |/user/getMoneyLog.html                        |/gateway-api/v1/private/capital/deposit/list                                |√  |√       |√  |√      |
|    |To apply withdraw                |/assetWithdraw/applyWithdraw.html             |/gateway-api/v1/private/capital/withdraw/apply                              |√  |√       |√  |√      |
|    |To fetch withdraw num.           |/assetWithdraw/getWithdrawNum.html            |/gateway-api/v1/private/capital/withdraw/withdrawMessage                    |√  |√       |√  |√      |
|    |To get withdraw list             |/withdraw/getAddressList.html                 |/gateway-api/v1/private/capital/withdraw/whitelist/list                     |√  |√       |√  |√      |
|    |To cancel withdraw               |/assetWithdraw/cancelWithdraw.html            |/gateway-api/v1/private/capital/withdraw/cancel                             |√  |√       |√  |√      |
|    |To resend mail                   |/assetWithdraw/reSendMail.html                |/gateway-api/v1/private/capital/withdraw/reSendMail                         |√  |√       |   |√      |
|    |To modify withdraw               |/withdraw/modifyAddress.html                  |/gateway-api/v1/private/capital/withdraw/whitelist/del                      |√  |√       |   |√      |
|    |To change deposit address        |/charge/getChargeAddress.html                 |/gateway-api/v1/private/capital/deposit/getAddress                          |√  |√       |   |√      |
|    |To reset deposit address         |/charge/resetChargeAddress.html               |/gateway-api/v1/private/capital/deposit/resetAddress                        |√  |√       |   |√      |
|    |To comfirm withdraw              |/assetWithdraw/confirmWithdrawForApp.html     |/gateway-api/v1/private/capital/withdraw/confirm                            |√  |√       |   |√      |
|    |To get withdraw address list     |/assetWithdraw/getWithdrawAddressList.html    |/gateway-api/v1/private/capital/withdraw/whitelist/list                     |√  |√       |√  |√      |
|    |To open withdraw white status    |user/openWithdrawWhiteStatus.html             |/gateway-api/v1/private/account/user/open-withdraw-white-status             |√  |√       |√  |√      |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |                                 |                                              |                                                                            |   |        |   |       |
|    |                                 |                                              |                                                                            |   |        |   |       |
