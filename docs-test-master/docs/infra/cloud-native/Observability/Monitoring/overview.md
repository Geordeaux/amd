---
id: overview
title: Overview
---

Monitoring collects metrics, events, and metadata from Amazon Kubernetes service and application instrumentation.  
Our monitoring suites ingests that data and generates insights via dashboards, charts, and alerts. 

To collect metrics data, we use [Promethesus](https://prometheus.io/) to scrape and store metrics as time series data, and use third-party exporters to export existing metrics from third-party systems as Prometheus metrics.

To visualize the performance, availability, and health of applications and infrastructure, We create, explore and share all the collected data through beautiful, flexible dashboards with [Grafana](https://grafana.com/grafana/).

To notify the issues of applications and infrastructure, Alerting rules are defined in Prometheus and Prometheus send the alert to [alertManger](https://prometheus.io/docs/alerting/latest/alertmanager/) to handles alerts to make sure the issues are notified and meanwhile routed to the correct receiver integration