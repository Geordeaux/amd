---
id: cicd-dashboard-scripts
title: CICD Dashboard tool script instructions
---

## ios_cicd_job_record_batch_update_local.py

### Description

Update the serialized storage of the CICDJob object with the specified path (multiple parameters can be modified at one time)

1. If there is no file in the specified file path, create a new serialized file
2. If there is a file in the specified file path：
   1. First deserialize the file into Object
   2. Update the values of multiple specified attributes in turn
   3. Serialize again to the specified file path

3. When `printJob = true`, the execution script will print the serialized file of the specified path and output it in JSON format

### Usage example

1. Update the serialization of CICDJob object with the specified path

```shell
python3 ios_cicd_job_record_batch_update_local.py  \
	--filePath $ios_cici_record_file \
  --platform 0 \
  --app $app \
  --jobType $jobType \
  --jobName ${JOB_NAME} \
  --prowJobId ${PROW_JOB_ID} \
  --buildId ${BUILD_ID} \
  --repoName ${REPO_NAME} \
  --pullBaseRef ${PULL_BASE_REF} \
  --pullBaseSha ${PULL_BASE_SHA} \
  --status 3 \
  --startedTime "$formattedStartedTime"
```

2. Print the serialized file of the specified path and output it in JSON format

```shell
python3 ios_cicd_job_record_batch_update_local.py \
	--filePath $ios_cici_record_file \
	--printJob true
```

# ios_cicd_job_record_upload.py

## Description

It can extract the CICDJob serialized file of the specified path, upload it to the server, and store the data in the database

1. After the upload is complete, the server will automatically print the content returned

## Usage example

```shell
python3 ios_cicd_job_record_upload.py --filePath $ios_cici_record_file
```

# ios_cicd_job_record_update_local.py (optional)

### Description

Add the specified attribute value to the CICDJob serialized object of the specified file path

1. If there is no file in the specified file path, create a new serialized file
2. If there is a file in the specified file path:
   1. First deserialize the file into Object
   2. Update attributes according to `--key` and `--value`
   3. Serialize again to the specified file path

### Parameter

| Key        | Value                                                        |
| ---------- | ------------------------------------------------------------ |
| --key      | The attribute you want to update                             |
| --value    | Fill in the value you want to add to the CICDJob object      |
| --filePath | Specify the storage file path                                |
| --printJob | If true, will print the serialized file of the specified path and output it in JSON format |

### Usage example

Update the `platform` attribute of the serialized CICDJob object of the `$ios_cici_record_file` path to `0`, and reserialize and store it into this path

```shell
python3 ios_cicd_job_record_update_local.py --filePath $ios_cici_record_file --key platform --value 0
```

Print the serialized CICDJob object under the specified path

```shell
python3 ios_cicd_job_record_update_local.py --filePath $ios_cici_record_file --printJob true
```

## ios_cicd_job_record_update_sub_local.py 

### Description

In the specified file path, read the serialized CICDJob object and add a CICDSubJob to it

1. If there is already a CICDSubJob object with the same `subJobName` in the subJobs array of the CICDJob object, it will be overwritten

### Parameter

| Key               | Value                                                        |
| ----------------- | ------------------------------------------------------------ |
| --filePath        | The location of the serialization file                       |
| --subJobType      | subJob type(currently not required)                          |
| --startedTime     | CICDSubJob start time                                        |
| --failureType     | Type of failure                                              |
| --failureKeywords | Failed keywords                                              |
| --customInfos     | Custom information, if any, please fill in the json converted to string |

### Usage example

```shell
python3 ios_cicd_job_record_update_common --filePath $ios_cici_record_file \
    --subJobType 0 \
    --startedTime "2021-03-09 16:09:33" \
    --duration 10 \
    --subJobName "pull-ios-client-publish-account::podinstall" \
    --failureType 0 \
    --failureKeywords "None" \
    --customInfos "{}"
```