---
id: logging
title: Logging
---

## Overview

Currently, Kiali logs are not preserved after new app version release or when a pod restarts. So it is difficult to troubleshoot an application if the pod has already been terminated.

To resolve this issue, we have now ingested all K8S cluster pod logs to ElasticSearch and made them searchable via Kibana. So a developer now can easily debug web-ui application using logs.

## Kibana Dashboards

### FE K8S Prod Log
- K8S Clusters: prod-com, prod-hk, toolsnew
- [Kibana Dashboard](https://kibana-ecs-prod.bin.eslogs.net/internal/security/saml/capture-url-fragment#/?_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:now-15m,to:now))&_a=(columns:!(message),filters:!(),index:f3640ab0-69e1-11eb-881a-672de41fc675,interval:auto,query:(language:kuery,query:%27%27),sort:!()))
- Request access permission from Okta.

### FE K8S Dev Log
- K8S Cluster: dev-new
- [Kibana Dashboard](https://kibana-nonprod.toolsfdg.net/app/dashboards#/view/f902cf40-e0b7-11eb-a192-cb8179da4eba)
- Request access permission from Okta.

### FE K8S QA1 Log
- K8S Cluster: qa1
- [Kibana Dashboard](https://kibana-nonprod.toolsfdg.net/app/dashboards#/view/4f4b5750-e4a0-11eb-b03f-09bffa730479)
- Request access permission from Okta.
