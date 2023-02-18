---
id: pipeline-to-prowjob
title: Migrate Tekton Pipeline to Prowjob
sidebar_label: Pipeline -> Prowjob
---

## Why don't we use tekton pipeline anymore.

Though Tekton Pipelines project provides k8s-style resources for declaring CI/CD-style pipelines, it's really hard for us to maintain. We have to pass params from prowjob to tekton pipeline and from tekton pipeline to tekton task. It's complex and needs much more time to understand and maintain. We need a simple way to update and understand CI/CD.

![pipeline-to-prowjob](https://static.devfdg.net/static/mono-static/docs-ui/img/pipeline-to-prowjob.jpg)

Actually, one tekton pipeline can include multiple tekton tasks, but we found there are some performance issues(Disk) when shares data between two tasks. So we decided only to use one tekton task in a tekton pipeline.

Previously, for example, we have a lot of copy/paste codes between tasks.

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: orca-task-deploy-prod-static
  namespace: tekton-pods
spec:
  params:
  - ...
  resources:
    inputs:
    - name: src
      type: git
  stepTemplate:
    env:
    - ...
  steps:
    - name: write-git-changes
      ...
    - name: write-deploy-data
      ...
    - name: upload-to-prod-s3-from-tools
      ...
    - name: upload-to-prod-cn-from-tools
      ...
    - name: aws-sns
      ...
```

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: orca-task-deploy-prod-hk-hr
  namespace: tekton-pods
spec:
  params:
  - ...
  resources:
    inputs:
    - name: src
      type: git
  stepTemplate:
    env:
    - ...
  steps:
  - name: write-git-changes
    ...
  - name: write-deploy-data
    ...
  - name: upload-to-prod-cn-from-tools
    ...
  - name: deploy-prod-hk
    ...
  - name: aws-sns
    ...
```

There are only a few differences between these two tasks, but we have to copy and paste those steps whose functions are the same. Hard to maintain, right?

## `prow-ci` docker image

```yaml
postsubmits:
  mono/mono:
  - name: post-mono-deploy-gray-hr
    ...
    spec:
      containers:
      - image: 918323888678.dkr.ecr.ap-northeast-1.amazonaws.com/prow-ci
        ...
```

We put all the scripts into a docker image `918323888678.dkr.ecr.ap-northeast-1.amazonaws.com/prow-ci`, can be found at `infra/images/prow-ci`.

```
./infra/images/prow-ci
├── opt
│   ├── aws
│   │   ├── s3-upload-from-tools        // upload statics to ${cluster}/s3 from tools s3
│   │   ├── s3-upload-to-tools          // upload statics to tools s3
│   │   ├── sns                         // AWS SNS notification
│   │   ├── sns-from-hr                 // invoke sns by the data from hr
│   │   └── ...
│   ├── github-api                      // scripts beneath this directory will invoke github apis
│   │   ├── commit-compare              // get compare msgs between two commits
│   │   ├── commit-create               // create a commit
│   │   ├── commit-file-content         // get file content by commit
│   │   ├── post-submit-mono            // create the gray pr of an app in mono
│   │   ├── pr-comment                  // comment to a pr
│   │   ├── pr-create                   // create a pr
│   │   ├── pr-create-from-hr           // create a pr by the datas from hr
│   │   ├── pr-files                    // get files changed of a pr
│   │   ├── release-create              // create a github repo release
│   │   └── ...
│   ├── notification
│   │   ├── dashboard                   // to notify cicd-dashboard
│   │   └── ...
│   ├── tccli
│   │   ├── coscmd-upload-from-tools    // upload statics to tencent cos from tools s3
│   │   └── ...
│   └── ...
├── shared
│   ├── .gitconfig                      // git config
│   └── install.sh                      // install env dependencies
├── Dockerfile
└── OWNERS
```

## How to migrate to prowjob?

### Step 1. To get to know what a tekton Task do.

Such as `orca-task-deploy-prod-static`, this task will deploy the statics of a specified version to the production environment.

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: orca-task-deploy-prod-static
  namespace: tekton-pods
spec:
  params:
  - ...
  resources:
    inputs:
    - name: src
      type: git
  stepTemplate:
    env:
    - ...
  steps:
    - name: write-git-changes
      ...
    - name: write-deploy-data
      ...
    - name: upload-to-prod-s3-from-tools
      ...
    - name: upload-to-prod-cn-from-tools
      ...
    - name: aws-sns
      ...
```

### Step 2. Write corresponding scripts in `prow-ci`

There are five steps:

- write-git-changes
- write-deploy-data
- upload-to-prod-s3-from-tools
- upload-to-prod-cn-from-tools
- aws-sns

We can found that almost all the functions are implemented in `prow-ci`.

| tekton step | `prow-ci` script |
|--|--|
| `write-git-changes` and `write-deploy-data` | `github-api/pr-files` |
| `upload-to-prod-s3-from-tools` | `aws/s3-upload-from-tools` |
| `upload-to-prod-cn-from-tools` | `tccli/coscmd-upload-from-tools` |
| `aws-sns` | `aws/sns` |

### Step 3. Invoke those scripts in prowjob

So we can use the following prowjob to replace it.

```yaml
# locate at: infra/prow/jobs/prowjobs-deploy-prod.yaml
- name: post-mono-deploy-prod-static
  ...
  labels:
    git: "true"
  spec:
    containers:
    - image: 918323888678.dkr.ecr.ap-northeast-1.amazonaws.com/prow-ci
      command:
      - /bin/bash
      args:
      - -c
      - |
        set -e

        pr-files --postsubmit ${PULL_BASE_SHA} \
        | jq -r '.[].filename' \
        | rg '^infra/statics/prod/static.*-prod\.yaml$' \
        | while read filename; do
          printf "\n\n------------ start: ${filename} ------------\n"
          # variables
          ecr_repo_name=$(yq r ${filename} repo.name)
          release_name=$(yq r ${filename} repo.name)
          revision=$(yq r ${filename} repo.commitId)
          static_tar_name="${revision}"

          # upload to s3 and flush cdn
          printf "\nStep 1. Upload to s3 from tools\n"
          time s3-upload-from-tools --path_to_bucket ${ecr_repo_name} --static_tar_name "${static_tar_name}.tar" --flush_cdn true

          # sns
          printf "\nStep 2. Notification\n"
          time sns -m "【static-prod 发版成功】${ecr_repo_name}: ${revision}"

          # dashboard
          printf "\nStep 3. Dashboard\n"
          time dashboard --name "${release_name}" --revision "${revision}"
        done
```

The label is required if we want to use the scripts in `github-api`. Prowjob will insert corresponding env when it detects the label configs.

```yaml
labels:
  git: "true"
```

```yaml
# locate at: infra/prow/config.yaml
- labels:
    git: "true"
  env:
  - name: GITHUB_USER
    valueFrom:
      secretKeyRef:
        name: git-credentials
        key: username
  - name: GITHUB_PASSWORD
    valuefrom:
      secretKeyRef:
        name: git-credentials
        key: password
```

### Step 4. Test prowjob

The last step is to trigger the prowjob to test if it works fine. Once the prowjob configs are merged into `master`, it will be automatically applied by the `config_updater`.

```yaml
# locate at: infra/prow/plugins.yaml
config_updater:
  maps:
    infra/prow/config.yaml:
      name: config
    infra/prow/plugins.yaml:
      name: plugins
    infra/prow/jobs/**/*.yaml:
      name: job-config
      gzip: true
```

> Notice that if we updated a prowjob, `rerun` will not take effect. Because `rerun` will always use the config created when trigger.

