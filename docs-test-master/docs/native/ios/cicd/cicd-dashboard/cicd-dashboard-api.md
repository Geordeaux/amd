---
id: cicd-dashboard-api
title: API & Database Documentation
---

## API

`POST` http://cicd-data-stream.cicd-k8s.toolsfdg.net/api/v1/fe

## Description

> The parameter name is the camel case format of the field in the database

- Request Body e.g.

```json
{
    "platform": 0,
    "app": 1,
    "jobType": 4,
    "jobName": "xxxx/jobname",
    "pr": "github.com/pull_request/4",
    "prowJobId": "prowJobId",
    "repoName": "qa/cd-agent",
    "pullBaseRef": "master",
    "pullBaseSha": "fcdyeiwr34",
    "pullRefs": "refs",
    "pullNumber": "5",
    "repoOwner": "fe",
    "logLink": "https://prow.toolsfdg.net/view/s3/toolsnew-fe-prow-logs/logs/post-ios-client-i18n-publish/1382578081792790528",
    "author": "harper.hu@binance.com",
    "startedTime": "2020-01-32 21:32:12",
    "buildId": "12345",
    "status": 5,
    "duration": 42111,
    "packageSize": 567,
    "packageLink": "huojnmlklink",
    "machineIp": "172.0.0.1",
    "machineXcodeVersion": "3.6",
    "failureType": 2,
    "failureKeywords": "connection",
    "customInfos": "customInfos",
    "subJobs": [{
        "subJobType": 2,
        "startedTime": "2020-01-32 21:32:12",
        "duration": 42111,
        "subJobName": "xxscd",
        "failureType": 2,
        "failureKeywords": "failed",
        "customInfos": "failed"
    },{
        "subJobType": 3,
        "startedTime": "2020-01-32 21:32:12",
        "duration": 42111,
        "subJobName": "xxscd",
        "failureType": 28,
        "failureKeywords": "failed",
        "customInfos": "failed"
    }]
}
```

> The `jobName` of the subjob can be omitted when requesting, the server will fill it with the `jobName` of the main job by default.

- Success response e.g.

```json
{
    "code": 0,
    "message": "",
    "data": 247,
    "ok": true
}
```

- Faild response e.g.

```json
{
  "code": Specific error code,
  "message": "Specific error message string"
}
```

## Parameter

### Enumerations and constants

[Enumerations and constants](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=63482335)

## Database field

### Table 1: fe_cicd_job

| key                   | type     | description                                                  | e.g.                                                         |                                 |
| --------------------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------- |
| id                    |          |                                                              |                                                              |                                 |
| created_time          |          |                                                              |                                                              |                                 |
| platform              | Int      | Platform(android or iOS or Web)                              | [Enumerations and constants](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=63482335) |                                 |
| app                   | Int      | AppHost app id                                               |                                                              |                                 |
| job_type              | Int      | Prow job type                                                | 0 periodic<br />1 presubmit<br />2 postsubmit                |                                 |
| job_name              | CHAR     | Prow job name                                                | pull-ios-client-publish                                      |                                 |
| pr                    | CHAR     | PR link                                                      |                                                              |                                 |
| prow_job_id           | CHAR     | Prow job id                                                  | 1ce07fa2-0831-11e8-b07e-0a58ac101036                         | 不能为空                        |
| build_id              | CHAR     | Unique build number for each prow job run.                   | 1364726971270959104                                          | 不能为空                        |
| repo_name             | CHAR     |                                                              | ios-client                                                   |                                 |
| pull_base_ref         | CHAR     |                                                              | master                                                       |                                 |
| pull_base_sha         | CHAR     |                                                              | Git SHA of the base branch. 123abc                           |                                 |
| pull_refs             | CHAR     |                                                              |                                                              |                                 |
| pull_number           | CHAR     | Pull request number.                                         | 5                                                            |                                 |
| repo_owner            | CHAR     |                                                              |                                                              |                                 |
| log_link              | CHAR     | Detial link of prow job                                      | https://prow.toolsfdg.net/view/s3/toolsnew-fe-prow-logs/logs/post-ios-client-i18n-publish/1382578081792790528 |                                 |
| author                | CHAR     |                                                              |                                                              |                                 |
| started_time          | Datetime | Start time                                                   |                                                              |                                 |
| status                | Int      | 0 - triggered<br />1- pending <br />2 - success <br />3 - failure<br />4 - aborted<br />5 - error |                                                              | Currently only 2 and 3 are used |
| duration              | Int      | Job execution time                                           | 100000                                                       |                                 |
| package_size          | Int      | The size of the packaged product (if has) (k is the unit)    | 2000000                                                      |                                 |
| package_link          | CHAR     | The link of the packaged product (if has)                    |                                                              |                                 |
| machine_ip            | CHAR     | machine IP(e.g. iOS Mac IP)                                  | xx.xx.xx.xx                                                  |                                 |
| machine_xcode_version | CHAR     |                                                              |                                                              |                                 |
| failure_type          | Int      | Reason for failure                                           | 0: success 1: unknown ...                                    |                                 |
| failure_keywords      | CHAR     | Failed keywords                                              | network need bundle install ...                              |                                 |
| custom_infos          | TEXT     | JSON for expansion Can store additional data (currently does not need to be displayed on the chart) |                                                              |                                 |

### Table 2: fe_cicd_sub_job

| key              | type     | description                                                  | e.g.                                                         |      |
| ---------------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| id               |          |                                                              |                                                              |      |
| created_time     |          |                                                              |                                                              |      |
| sub_job_type     | Int      | Not used yet                                                 |                                                              |      |
| sub_job_name     | CHAR     | 打包任务的子阶段名称，eg 格式：{{ job_name }}::{{ sub_job_abbreviation}} | pull-ios-client-publish::pod_install<br />pull-ios-client-publish::build<br />pull-ios-client-publish-account-a::pod_install<br />pull-ios-client-publish-account-a::build ... |      |
| job_name         | CHAR     | Job name of main job(Table 1)                                |                                                              |      |
| started_time     | Datetime | Start time                                                   |                                                              |      |
| duration         | Int      | Subjob execution time                                        | 1000                                                         |      |
| failure_type     | Int      | Reason for failure                                           | 0: success 1: unknown ...                                    |      |
| failure_keywords | CHAR     | Failed keywords                                              |                                                              |      |
| custom_infos     | TEXT     | JSON for expansion Can store additional data (currently does not need to be displayed on the chart) |                                                              |      |
| job_id           | CHAR     | job_record_id                                                |                                                              |      |