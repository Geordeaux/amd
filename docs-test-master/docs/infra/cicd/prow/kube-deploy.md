---
id: kube-deploy
title: kube-deploy
---

## What is `kube-deploy`
this is a prow external-plugin for deploy the match changes to our cluster  
once PR merged, will trigger `kube-deploy` to deploy the change files

the config is in `infra/prow/plugins.yaml`

## How to use
1. `/deploy check` comment this command in PR will trigger check the change files
check will use `kubectl --dry-run=server` to check for cluster

2. `/deploy redeploy` comment this command in PR will trigger redeploy the change files