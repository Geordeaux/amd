---
id: api-mp-ide
title: Api for IDE
sidebar_label: IDE
---
import Mermaid from '@theme/Mermaid'



## IDE
## API_HOST

* dev: https://developers.devfdg.net/
* qa: https://developers.qa1fdg.net/
* prod: https://developers.binance.com/


## OAuth login

<Mermaid chart={`
sequenceDiagram
  IDE->>+OAuth: Redirect to OAuth
  OAuth->>IDE: Redirect to redirectUri with code
  IDE->>API: GET /bapi/mp/v1/management/sessions?code=[insert code here]
`}></Mermaid>

1. IDE redirect to https://accounts.devfdg.net/en/oauth/authorize?response_type=code&client_id=00JYpmtGNw&redirect_uri=https://developers.devfdg.net/en/mp/management/oauth 
2. OAuth redirect back to the `redirect_uri`
3. IDE handle the callback from OAuth and send code from OAuth to IDE.
4. IDE send a `GET` /bapi/mp/v1/management/sessions?code=[insert code here]
5. Backend return a response called `session`.
6. Ide forward the `session` in header `Authorization: Bearer [session]` for each of the api.

## Sesssion

### `GET` /bapi/mp/v1/management/sessions

Example: /bapi/mp/v1/management/sessions?code=test123test&redirect_uri=binance://xxxxx

* query
```ts
{
  code: string
  redirect_uri: string
}
```

* response
```ts
{
  sessionToken: string // 4H5JRJ559675EQ7ZKaDDyU
  expiresIn: number // 1 day in milliseconds
}
```

###  `DELETE` /bapi/mp/v1/management/sessions

delete session

* response
```ts
boolean
```


## Upload

### `POST` /bapi/mp/v1/management/applicationVersions

create a new app version

```ts
interface Request {
  applicationId: number,
  version: string // semver
  description: string
  sdkVersion: string
  fileKey: string // get this fileKey from presignUrl endpoint
}
```

> To get fileKey, call the `/bapi/mp/v1/management/upload/application`

<Mermaid chart={`
sequenceDiagram
  Browser->>+Api: Post /bapi/mp/v1/management/upload/application { name, applicationId }
  Api->>Browser: { fileKey, url }
  Browser->>S3: PUT url (upload file)
  Browser->>Api: { applicationId, version, fileKey, description, sdkVersion)
`}></Mermaid>


* response

```ts
interface Version {
  "createdAt": number,
  "updatedAt": number,
  "id": number,
  "version": string,
  "revision": string,
  "sdkVersion": string,
  "isExperience": false,
  "description": string,
  "downloadUrl": null | string,
  "status": 'DEVELOP' | 'AUDITING' | 'APPROVED' | 'REJECTED' | 'PUBLISHED',
  "publishedTimestamp": number
}

interface Response {
  online: Version | null; // 线上版本
  audit: Version | null; // 审核版本
  develop: Array<Version> // 开发中版本
}
```
### `POST` /bapi/mp/v1/management/upload/application

get a s3 presigned url from server to upload your file

```ts
interface Request {
  name: string // fileName
  applicationId: number
}

interface Response {
  url: string 
  fileKey: number
}
```

## User

### `GET` /bapi/mp/v1/management/user/applications

get all editable applications

```ts
Array<{
  {
    appId: string,
    avatarUrl: string | null,
    createdAt: number,
    description: string,
    developer: string, // developer name
    id: number,
    isActive: boolean,
    name: string,
    updatedAt: number,
  }
}>
```

### `GET` /bapi/mp/v1/management/user/roles

Example: /bapi/mp/v1/management/user/roles?applicationId=1

get all roles

* params (optional)

```ts
{ 
  applicationId?: number,
  developerId?: number
}
```

```ts
Array<{
  {
    appId: string,
    avatarUrl: string | null,
    createdAt: number,
    description: string,
    developer: string, // developer name
    id: number,
    isActive: boolean,
    name: string,
    updatedAt: number,
  }
}>
```
