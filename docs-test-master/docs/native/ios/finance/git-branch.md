---
id: git-branch
title: 開發分支說明
---

## 功能開發

目前在 Git 上開發，採用的是 [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=The%20overall%20flow%20of%20Gitflow,branch%20is%20created%20from%20main&text=When%20a%20feature%20is%20complete%20it%20is%20merged%20into%20the,branch%20is%20created%20from%20main) 開發流程，主要分支有兩個

* master: `master`，主要穩定分支，不在此分支上直接開發
* develop: `bnbfly`，主要開發分支，各單位會從此分支上另開分支出去，再做開發

當每一期的 JIRA 票開了之後，各單位就會以票號作為分支名稱，在 `bnbfly` 上開分支出去開發。

例如 [FUT-15716](https://jira.toolsfdg.net/browse/FUT-15716) 為 0930 的 JIRA ticket，futures 團隊就會新增 `FUT-15716-FUTURES/bnbfly` 分支給團隊內部開發，並且不定時的跟 `bnbfly` 同步。

* 大約是需求凍結之後一到兩天會新增分支

## 提測

第一波提測前，會從分支 `FUT-15351-Finance-Archive` 建立對應的 [pull request](https://git.toolsfdg.net/fe/ios-client/pull/13319) (from `FUT-15351-Finance-Archive` to `bnbfly`)，PR 的標題為 `WIP: [Finance][2.36.0][Archive]`，用來合併各單位 (e.g. futures, spot and margin) 的開發分支。

> 注意：archive 用的 PR 是用來出測試包的，不能合併，出包方式可參考 [CI / CD 流程](./cicd-flow)

發布提測，是為了讓 QA 及 UED 團隊測試並驗收。主要會有兩次，時間點可參考[發版流程](./release-flow)。須在**提測前一天的下班前**，確保當期功能都已經合併到各單位的開發分支上。

`bin_alert_bot` 會將提測包資訊，發佈在 `sig-ios-cicd` 與 `financial-ios-cicd` 群組中

![](https://static.devfdg.net/image/julia/package-message.png)

## 發布

在正式發佈大約一週前時，會從 `master` 分支新增 release 分支，名稱會以當期的 JIRA release ticket 命名，例如 2.36.0 版本的 JIRA ticket: [APP-4860](https://jira.toolsfdg.net/browse/APP-4860)，對應的就是 `APP-4860-2_36_0-release` 分支。

正式發佈會以灰度設定，逐漸讓使用者下載、安裝。需要在 code freeze 前，確保當期功能，及先前測試或驗收發現的問題，都已經修復並且合併到 release 分支上。

但凡在發版前一天 code freeze 之後才提交的 PR，需先在 `iOS Flight Schedule（发车群）` 群中提交申請，提交格式為：

```
xxx 申请合并：
【问题现象】：
【影响范围】：
【问题原因】：
【解决方案】：
【修改风险】：
【是否阻断】：
【PR】:
```



## 熱修復 (Hot-Fix)

若在正式發佈後，存在需緊急修復的問題，會從當期的 release 分支新增分支。

Hotfix 分支名稱對應 JIRA hotfix ticket，以 [APP-4840](https://jira.toolsfdg.net/browse/APP-4840) (2.35.1 hotfix) 為例，命名為 `APP-4840-2_35_1-hotfix`。再透過正式發佈讓使用者下載。

Hotfix 的 PR，需先在 `iOS Flight Schedule（发车群）` 群中提交申請，提交格式為：

```
xxx 申请 x.xx.x hotfix
【问题现象】
【影响范围】
【问题原因】
【修改方案】
【修改风险】
【是否阻断】
【PR地址】
```

