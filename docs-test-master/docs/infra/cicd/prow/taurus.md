---
id: taurus
title: Workflow with TCM
sidebar_label: tcm-agent
---

## How to create a `tcm-agent` prowjob?

### Step 1. Create a `TCM` task template.

So far we support two types of Task.

- automatic project
- manual project

Click the [new Task](https://qcenter.k8s.qa1fdg.net/projectmanager#/task/template), then input the name and the owner, and relate a project to the task.

> Caution: The name of the task template should be semantic! E.g. `${app_name}:${module}:${tests}`, `${app_name}:all`

![create-task-template](https://static.devfdg.net/static/mono-static/docs-ui/img/create-task-template.png)


### Step 2. Create the corresponding prowjob

```yaml
- name: tcm-accounts-ui
  branches:
  - master
  agent: tcm-agent
  rerun_command: "/tcm accounts-ui:all"
  trigger: "(?mi)^/tcm accounts-ui:all\\s*$"
  pipeline_run_spec:
    pipelineRef:
      name: "accounts-ui:all"
```

## Architecture

![arch](https://static.devfdg.net/static/mono-static/docs-ui/img/taurus-arch.png)

