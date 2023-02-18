---
id: started
title: Getting Started
---

The polaris system enables overall performance score of your applications so that you can rapidly get the performance of your applications and the observability of every metrics by the metrics score 

## Step 1: Storing your performance data to ES

If your data has been stored in certain ES, go to **Step 2**, or Use these steps to store the performance data to ES 
1. upload your performance data use the [new upload interface](https://docs.fe.devfdg.net/docs/infra/cloud-native/Project/Universal-sensor-data/quickStart)
2. get a kafka topic from infra team @alex @selena @daniel
3. infra team asks the devops team to add a logstash to consume the message to certain ES 
4. check that the performance data has been stored into the ES by kibana
   1. add `Kibana | saas-fe-log` from your okta home
   2. click the icon to reach out to the kibana to check your performance data stored into the ES

## Step 2: Defining the Metrics and its weight

1. Define the metrics that applies to your overall performance score (polaris score) according to [metrics](https://docs.fe.devfdg.net/docs/infra/cloud-native/Project/Polaris/metrics). Of course you can use metrics that are not list in [metrics](https://docs.fe.devfdg.net/docs/infra/cloud-native/Project/Polaris/metrics) 
2. Define weight of every metrics
3. Decide what metric scores you want to store for further diagnose the web performance issue
4. Calculate the polaris score and store them into the certain ES.

## Step 3：Visualizing the Polaris score

1. Set up the grafana dashboard with the polaris score
2. TBC...
