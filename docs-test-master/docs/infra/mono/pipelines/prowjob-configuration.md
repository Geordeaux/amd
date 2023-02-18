---
id: prowjob-configuration
title: Publish Prowjob Configuration
---


This article will describe in detail the meaning and usage scenarios of prowjob.

You can find prowjob configuration source here `infra/prow/jobs/prowjobs-pr-publish-mono.yaml`.

A simple example:

![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/prowjob-example.png)

You can follow the steps below:
1. Change value `help-center-ui` to your app name, change field `github_users` to the owner. 
2. Focus on the `publish-mono` in the `command` field, look down for detailed introduction.

> The `--${key} ${value}` in the code is the parameter of prowjob. Also some parameters are like switch, you only need to declare to turn it on or off, just like the `--enable_pr_injection` above

> [CICD Management](https://cicd-manage-fe.toolsfdg.net) will help you do this more easily.


---
## App Related

### app_name(Required)

Declare the name of the app, such as `home-ui`.

### app_owner

Declare the owner of the app, the default is fe.

### path_to_app

Declare the path of app, the default is apps.

You can declare a special path for some applications with app_name, such as `apps/legacy` with app_name `exchange-ui`, so the final app path is `apps/legacy/exchange-ui`.

---
## Upload Statics Related

### flush_cdn

Used by aws s3 to upload static, declare to if flush cdn, the default is false.

### disable_uploading_statics

If set this, will not upload static to aws s3. The default is enabled.

In most cases, you don’t need to set it, but make sure that your app build production path is `dist`.

If production path is not `dist`, please set with `static` to change file mapping.

Example:
```yaml
--static "build => dist"
# build is now your local build path
# The detailed introduction is in the `static` area
```

### static_suffix

Define the suffix name of the static resource uploaded to aws s3.

### static

Declare a mapping from the local resource path to the remote resource path, the default is `dist/client/static => static`

---
## Sentry Related

### enable_sentry

Declare to enable sentry, the default is disabled.

### enable_pt

Use Stress testing, the default is disabled.

### sentry_dist

Declare the path of the sentry static file, the default is `dist`.

### sentry_url_prefix

Declare the url prefix, it will be exported to environment variables.

---
## Create Gray Pr

### enable_creating_gray_pr

Declare to open the prod gray environment, [use case](https://docs.fe.devfdg.net/docs/infra/mono/create-app#5how-to-deploy-your-application), the default is disabled.

### enable_creating_testnet_pr

Declare to enable the testnet environment to test pr, the default is disabled.

---
## Build

### disable_building

Declare to disable `yarn build`, services like `node` do not need to be compiled, the default is enabled.


### esbuild

Declare to enable esbuild, the default is disabled.

### enable_use_remote_cache

After declare, if the application dependency has not changed, the remote cache will be used. The default is disabled.

### build_commands

Declare to pass in your custom build commands, the default is  `infra/images/prow-ci/opt/publish/build-mono`.

---
## Build Cache

### disable_rebuilding_cache

Declare to disabled rebuild cache. If your app needs to be compiled for a long time, you can close it. The default is enabled.

### prod_namespace

Declare the namespace of prod env, the default is `prod`.

### prod_cluster

Declare which cluster to use, all cluster names can be found in the `infra/k8s-resources` directory. The default is prod-com.

### gray_multi_pr_path

Declare multiple cluster deployment, this means you want to deploy source to multiple production clusters.

### build_cache_local_path

Declare to pass in a custom local cache path, the default is `web/$appName/node_modules`.

---
## Upload Ecr

### disable_buildkitd_remote

Declare to disable caching build docker, If your app compilation speed is not slow, can turn it on. The default is disabled.

### disable_deploying

Declare to disable deploy, includes docker image and helm release.

### path_to_dockerfile

Declare to pass in a custom dockerfile path, like `--path_to_dockerfile apps/shared/Dockerfile.spa`. The default is `web/apps/shared/Dockerfile`.

### path_to_context

Declare to pass in a custom file path context, the default is `$(pwd)`

---
## Deploy Helm Release

### enable_pr_injection

Declare to enable the use of FeHeader to the page to access your pr content, the default is disabled.

### disable_qa1

Declare to disable qa1 environment, the default is enabled.

### disable_dev

Declare to disable dev environment, the default is enabled.

### deploy_values

Declare to pass in the value your want modify when helm release, like ` --deploy_values "--set health=/health-check`. It will help you set the value of `health` field to `/health-check`
