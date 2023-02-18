---
id: online_issues_location_and_tracking
title: Online issues location and tracking
hide_title: true
---

# Online issues location and tracking

### Purpose

We can different tools to get our daily log. 

However, They have their own purpose and situations

### Content

- Firebase → Catch bugs log from our users.
- Logan → Track  the state of end-to-end network and HappyWss connection.
- 神策 → Check user's behavior.

Search the state of network state for example：

1. Ask user upload logan manually
    1. personal setting
    2. Help & Support
    3. System feedback
    4. Acquire user's uid：10010396
2. Open Kibana : [https://kibana-ecs-prod.bin.eslogs.net/app/discover](https://kibana-ecs-prod.bin.eslogs.net/app/discover)

   ![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/online_issues_location_and_tracking_1.png)
    

3. Choice binance-gateway-service after you get into the page.
Let's search by user's uid.

   ![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/online_issues_location_and_tracking_1.png)

4. After getting bnc-uuid, You could search the logan's log that user just uploaded.




Others

- Check LCP time-consumed on network requests.

   ![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/online_issues_location_and_tracking_3.png)



[Ref](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=51979924)