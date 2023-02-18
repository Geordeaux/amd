---
id: web
title: The Flow of Publishing Web Apps
sidebar_label: Web
---

## CI

### 1. Restore and Rebuild Cache

> Mostly, we only store the `node_modules/.cache` which produced by webpack

Bot will download the cache directory(can be overrided by `--build_cache_s3_uri`) from AWS tools s3. After the build phase, the cache will be rebuilt and then override the previous one.

### 2. Build App

For those apps in `mono`, the bot will execute `mono build -p ${app_name}` to build all the dependant libs(analysed from `apps/${app_name}/BUILD.bazel`) and the app itself. 

### 3. Build Docker Image

Then the bot will use [Kaniko](https://github.com/GoogleContainerTools/kaniko) to build a docker image with the [apps/shared/Dockerfile](https://git.toolsfdg.net/mono/mono/blob/master/apps/shared/Dockerfile). The image will be uploaded to AWS ECR.

### 4. Upload Statics

For those statics(html, css, js), firstly bot will zip them and upload the zipped file to the tools AWS S3. When do the CD of a app, bot will download from that path and upload the unzipped contents to the specified(dev, qa, prod) AWS S3.

## CD

It's very simple to deploy our services on different clusters. Only need two steps.

### 1. Upgrade Helm Release

On the dev or the qa clusters, we deploy our services directly executing `helm upgrade`.

```bash
helm --kube-context bin-dev-fe-eks -n web-ui upgrade -i ${release_name} binance/web-ui -f ./values.yaml
```

But on the gray or the prod clusters, we use the [Helm Operator](https://docs.fluxcd.io/projects/helm-operator/en/stable/) which will persist our customized values to a yaml config to do the deployment.

```yaml
apiVersion: helm.fluxcd.io/v1
kind: HelmRelease
metadata:
  name: <app-name>-prod
  namespace: prod
spec:
  chart:
    repository: https://public.bnbstatic.com/fe-helm/charts
    name: web-ui
    version: 1.1.0
  rollback:
    enable: false
  releaseName: <app-name>-prod
  values:
    service:
      enabled: false
    istio:
      enabled: false
      subsetName: prod
    canary:
      enabled: false
    github:
      repoOwner: mono
      repoName: mono
    nameOverride: <app-name>
    image:
      repository: 918323888678.dkr.ecr.ap-northeast-1.amazonaws.com/fe/<app-name>
      tag: e7597ab006fae50eea36b8f7866c187e19fa9f07
    containerPort: 3000
    health: /health-check
    envFrom:
    - configMapRef:
        name: common-ui
    - configMapRef:
        name: <app-name>
    hpa:
      enabled: true
      minReplicas: 2
```

### 2. Upload statics

Then download the statics which generated during [Upload Statics](#4-upload-statics) and upload the unzipped contents to the specified(dev, qa, prod) AWS S3.
