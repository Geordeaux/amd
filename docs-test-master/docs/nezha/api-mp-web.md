---
id: api-mp-web
title: Api for Management Portal
sidebar_label: Management
---
import Mermaid from '@theme/Mermaid'


## API_HOST

* dev: https://developers.devfdg.net/
* qa: https://developers.qa1fdg.net/
* prod: https://developers.binance.com/

## OAuth login

<Mermaid chart={`
sequenceDiagram
  Browser->>+OAuth: Redirect to OAuth
  OAuth->>Browser: Redirect to redirectUri with code
  Browser->>API: GET mp-api/v1/management/sessions?code=[insert code here]
`}></Mermaid>

1. Browser redirect to https://accounts.devfdg.net/en/oauth/authorize?response_type=code&client_id=00JYpmtGNw&redirect_uri=https://developers.devfdg.net/en/mp/management/oauth 
2. OAuth redirect back to the `redirect_uri`
3. Browser handle the callback from OAuth and send code from OAuth to browser.
4. Browser send a `GET` /bapi/mp/v1/management/sessions?code=[insert code here]
5. Backend will save a cookies called `session` in browser.

## Session

###  `GET` /bapi/mp/v1/management/sessions

Example: /bapi/mp/v1/management/sessions?code=test123test

It will be saved in cookie. Example: `cookie: session=4H5JRJ559675EQ7ZKaDDyU;`

* query
```ts
{
  code: string
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

## Developers

### `GET` /bapi/mp/v1/management/developers

get developers list

* query
```ts
// TODO pagination
interface Query {
}
```

* response (same as GET)

```ts
Array<{
  createdAt: number,
  updatedAt: number,
  id: number,
  name: string,
  clientId: null | number,
  owner: {
    id: number,
    email: string,
  },
  settings: {
    oauth: {
      scope: string,
      clientId: string,
      redirectUri: string
    },
    whitelist: string[],
  }
}>
```

### `POST` /bapi/mp/v1/management/developers

create a developer account

* body

```ts
{
  name: string
}
```

* response 

```ts
Array<{
  createdAt: number,
  updatedAt: number,
  id: number,
  name: string,
  clientId: null | number,
  owner: {
    id: number,
    email: string,
  },
  settings: {
    oauth: {
      scope: string,
      clientId: string,
      redirectUri: string
    },
    whitelist: string[],
  }
}>
```

### `PUT` /bapi/mp/v1/management/developers/:id/settings

update a developer settings

* body

```ts
{
  oauth?: {
    clientId: string
    redirectUri: string
    scope: string
  }
  whitelist?: string[]
}
```

* response 

```ts
{
  createdAt: number,
  updatedAt: number,
  id: number,
  name: string,
  clientId: string | null,
  settings: {
    oauth: {
      scope: string,
      clientId: string,
      redirectUri: string
    },
    whitelist: string[]
  }
}
```

### `GET` /bapi/mp/v1/management/developers/:developerId/users

get all users for the developer account

Example: `/mp-api/v1/management/developers/1/users`

* response 

```ts
Array<{
  "id": string,
  "email": string,
  "role": "Owner" | "Editor" | "Viewer"
}>
```

### `POST` /bapi/mp/v1/management/developers/:developerId/users

invite user to the developer account

Example: `/mp-api/v1/management/developers/1/users`

* body 
```ts
{
  "email": string,
  "edit": boolean
}
```
* response (same as get)

```ts
Array<{
  "id": string,
  "email": string,
  "role": "Owner" | "Editor" | "Viewer"
}>
```

### `DELETE` /bapi/mp/v1/management/developers/:developerId/users

delete user from the developer account

Example: `/mp-api/v1/management/developers/1/users`

* body 
```ts
{
  "userId": number, // userId
}
```
* response (same as get)

```ts
Array<{
  "id": string,
  "email": string,
  "role": "Owner" | "Editor" | "Viewer"
}>
```

## Applications

### `GET` /bapi/mp/v1/management/applications

get application list

Example: `mp-api/management/applications?developerId=1`

* query
```ts
interface Query {
  developerId: number
}
```

* response

```ts
Array<{
  appId: string',
  avatarUrl: string|null,
  createdAt: number,
  currentAuditingVersion: ApplicationVersion |null,
  currentPublishedVersion: ApplicationVersion| null,
  description: string,
  isActive: boolean,
  name: string,
  updatedAt: number,
}>
```

### `POST` /bapi/mp/v1/management/applications

create a new mini program

* body

```ts
{
  "developerId": number,
  "name": string,
  "description": string,
  "avatarFileKey": string
}
```

> To get avatarFileKey, call the `/mp-api/v1/management/upload/image`

<Mermaid chart={`
sequenceDiagram
  Browser->>+Api: Post /mp-api/v1/management/upload/image { name, developerId }
  Api->>Browser: { fileKey, url }
  Browser->>S3: PUT url (upload file)
  Browser->>Api: { createdAt, updatedAt, id, name, description, avatarUrl, isActive }
`}></Mermaid>


* response (same response as GET)

```ts
Array<{
  "createdAt": number,
  "updatedAt": number,
  "appId": string,
  "name": string,
  "description": string,
  "avatarUrl": null | string,
  "isActive": boolean,
  "qrcode": {
    "online": string,
    "experience": string,
    "auditing": string
  },
  "developmentSettings": {
    "backgroundFetchUrl": string,
    "certificates": Array<{"domain": "google.com","keys": Array<string>}>,
    "preFetchUrl": string,
    "serviceDomain": string[]
    "webViewDomain": string[],
  }
}>
```

### POST /bapi/mp/v1/management/upload/image

```ts
interface Request {
  name: string // fileName
  developerId: number
}

interface Response {
  fileKey: string
  url: string // send a PUT to this url with file to upload to s3
}
```

### `PUT` /bapi/mp/v1/management/applications/:id

Update an application

Example: `/bapi/mp/v1/management/applications/1`


* body

```ts
{
  "name"?: string,
  "description"?: string,
  "avatarFileKey"?: string
}
```

* response

```ts
{
  "createdAt": number,
  "updatedAt": number,
  "appId": string,
  "name": string,
  "description": string",
  "avatarUrl": string | null,
  "isActive": false,
  "qrcode": {
    "online": string,
    "experience": string,
    "auditing": string
  },
  "developmentSettings":{
    "backgroundFetchUrl": string,
    "certificates": Array<{"domain": "google.com","keys": Array<string>}>,
    "preFetchUrl": string,
    "serviceDomain": string[]
    "webViewDomain": string[],
  }
}
```

### `PUT` /bapi/mp/v1/management/applications/:id/status

Update an application status

Example: `/mp-api/v1/management/applications/1/status`


* body

```ts
{
  "isActive": boolean
}
```

* response

```ts
{
  "createdAt": number,
  "updatedAt": number,
  "appId": string,
  "name": string,
  "description": string",
  "avatarUrl": string | null,
  "isActive": false
  "qrcode": {
    "online": string,
    "experience": string,
    "auditing": string
  },
  "developmentSettings": {
    "backgroundFetchUrl": string,
    "certificates": Array<{"domain": "google.com","keys": Array<string>}>,
    "preFetchUrl": string,
    "serviceDomain": string[]
    "webViewDomain": string[],
  }
}
```

### `PUT` /bapi/mp/v1/management/applications/:id/settings

Update an application development settings

Example: `/bapi/mp/v1/management/applications/1/settings`


* body

```ts
{
  "backgroundFetchUrl"?: string,
  "preFetchUrl"?: string,
  "serviceDomain"?: Array<string>
  "webViewDomain"?: Array<string> 
}
```

* response

```ts
{
  "createdAt": number,
  "updatedAt": number,
  "appId": string,
  "name": string,
  "description": string",
  "avatarUrl": string | null,
  "isActive": false
  "qrcode": {
    "online": string,
    "experience": string,
    "auditing": string
  },
  "developmentSettings": {
    "backgroundFetchUrl": string,
    "certificates": Array<{"domain": "google.com","keys": Array<string>}>,
    "preFetchUrl": string,
    "serviceDomain": string[]
    "webViewDomain": string[],
  }
}
```

### `GET` /bapi/mp/v1/management/applications/:applicationId/users

get all users for the developer account

Example: `/bapi/mp/v1/management/applications/1/users`

* response 

```ts
Array<{
  "id": string,
  "email": string,
  "role": "Owner" | "Editor" | "Viewer" | "Experience User"
}>
```

### `POST` /bapi/mp/v1/management/applications/:applicationId/users

invite user to the developer account

Example: `/bapi/mp/v1/management/applications/1/users`

* body 
```ts
{
  "userId": string,
  "edit": boolean
}
```
* response (same as get)

```ts
Array<{
  "id": string,
  "email": string,
  "role": "Owner" | "Editor" | "Viewer"
}>
```

### `POST` /bapi/mp/v1/management/applications/:applicationId/experience

add a user to experience the application

Example: `/bapi/mp/v1/management/developers/1/experience`

* body 
```ts
{
  "email": string,
}
```
* response (same as get)

```ts
Array<{
  "id": string,
  "email": string,
  "role": "Owner" | "Editor" | "Viewer" | "Experience User"
}>
```


### `DELETE` /bapi/mp/v1/management/applications/:applicationId/users

delete user from the developer account

Example: `/bapi/mp/v1/management/developers/1/users`

* body 
```ts
{
  "userId": number, // userId
}
```
* response (same as get)

```ts
Array<{
  "id": string,
  "email": string,
  "role": "Owner" | "Editor" | "Viewer" | "Experience User"
}>
```


### `POST` /bapi/mp/v2/management/apps/:applicationId/qr-code

Create QRCode for an application.

Example: `/bapi/mp/v2/management/apps/1/qr-code`

* Header
```ts
  Authorization: Bearer <session-token>
  Cookie: session=<session-token>
 ```
 Either one of the above headers is required for authenication purpose.
 
* body 
```ts
{
  "startPagePath": string, // start page path.
  "startPageQuery": string, // query params
}
```
* response

```ts
{
  "data": string, // QR code string
  "code": string,
  "success": boolean,
}
```
NOTE: Currently this API is supported with only oAuth login. Okta login is not yet supported.


## Version Management

### `GET` /bapi/mp/v1/management/applicationVersions

get all information about all versions of a specified app

Example: `/mp-api/v1/management/applicationVersions?applicationId=1`
* params

```ts
{ applicationId: string }
```

* response

```ts
interface Version {
  "createdAt": number,
  "updatedAt": number,
  "id": number,
  "version": string,
  "revision": string,
  "sdkVersion": string,
  "qrCode": string | null
  "isExperience": false,
  "description": string,
  "downloadUrl": null | string,
  "status": 'DEVELOP' | 'AUDITING' | 'APPROVED' | 'REJECTED' | 'PUBLISHED',
  "publishedTimestamp": number
  "developmentSettings": {
    "backgroundFetchUrl": string,
    "certificates": Array<{"domain": "google.com","keys": Array<string>}>,
    "preFetchUrl": string,
    "serviceDomain": string[]
    "webViewDomain": string[],
  },
  "appType": 'mini-program' | 'web-app',
  "webAppUrl": string | null
}

interface Response {
  online: Version | null; // 线上版本
  audit: Version | null; // 审核版本
  develop: Array<Version> // 开发中版本
}
```


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

> To get fileKey, call the `/mp-api/v1/management/upload/application`

<Mermaid chart={`
sequenceDiagram
  Browser->>+Api: Post /mp-api/v1/management/upload/application { name, applicationId, size }
  Api->>Browser: { fileKey, url }
  Browser->>S3: PUT url (upload file)
  Browser->>Api: { applicationId, version, fileKey, description, sdkVersion)
`}></Mermaid>


* response (same as GET)

```ts
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
  size: number// file.size
}

interface Response {
  fileKey: string
  url: string // send a PUT to this url with file to upload to s3
}
```

### `GET` /bapi/mp/v1/management/applicationVersions/:id/qrcode

get qrcode of an applicationVersion

* response

```ts
string // qrcode
```

### `DELETE` /bapi/mp/v1/management/applicationVersions/:versionId

delete an applciation version

* response (same as GET)

```ts
interface Response {
  online: Version | null; // 线上版本
  audit: Version | null; // 审核版本
  develop: Array<Version> // 开发中版本
}
```

### `PUT` | `DELETE` /bapi/mp/v1/management/applicationVersions/:versionId/audit

submit or withdraw audit

* response (same as GET)

```ts
interface Response {
  online: Version | null; // 线上版本
  audit: Version | null; // 审核版本
  develop: Array<Version> // 开发中版本
}
```

### `PUT` /bapi/mp/v1/management/applicationVersions/:versionId/publish

deploy the **approved version** to production

* response (same as GET)

```ts
interface Response {
  online: Version | null; // 线上版本
  audit: Version | null; // 审核版本
  develop: Array<Version> // 开发中版本
}
```

### `PUT` | `DELETE` /bapi/mp/v1/management/applicationVersions/:versionId/experience

set or cancel version as an experience version

* response (same as GET)

```ts
interface Response {
  online: Version | null; // 线上版本
  audit: Version | null; // 审核版本
  develop: Array<Version> // 开发中版本
}
```

### `GET` /bapi/mp/v1/management/applicationVersions/:versionId/publishedVersions

get last 5 published versions excluding the current online.

* response

```ts
Array<{
  "createdAt": number,
  "updatedAt": number,
  "id": number,
  "version": string,
  "revision": string,
  "qrCode": string | null
  "sdkVersion": string,
  "isExperience": boolean,
  "description": string,
  "downloadUrl": string,
  "status": "PUBLISHED",
  "publishedTimestamp": number
}>
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
    "name": "Owner",
    "permissions": {
      "code": [
        "own",
        "edit",
        "view",
      ],
      "objectId": number | string,
      "objectType": "application" |"developer",
    },
  }
}>
```

### `GET` /bapi/mp/v1/management/user/info

Example: /bapi/mp/v1/management/user/info

get user info

```ts
{
  "userId": number,
  "email": string,
  "developers": Developer[],
  "roles": Array<{
    name: string,
    permissions: Array<string>,
    objectId: number
  }>
}
```

# Golang service
The following APIs are implement in golang and we plan to migrate all APIs from NodeJS to golang.

**Note** The API host endpoint is the same as NodeJS service. However, golang service is under v2 path instead of v1.

## Applications

###  `GET` /bapi/mp/v2/management/apps/:appId

Example: `/bapi/mp/v2/management/apps/niHpLXzhjkocTrKvT1T7oQ?developerId=16`

Get app information for the given appId. Now only i18N contents in this API. developerId should be passed to check your permissions along with userId.

* response
```ts
{
  "data": {
    "appId": string,
    "i18nContents": Array<{
      "languageCode": string,
      "name": string,
      "description": string
    }>
  },
  "code": string,
  "success": boolean
}
```

Example for response:
```json
{
  "data": {
    "appId": "niHpLXzhjkocTrKvT1T7oQ",
    "i18nContents": [
      {
        "languageCode": "en",
        "name": "hello",
        "description": "spongebob"
      },
      {
        "languageCode": "zh-TW",
        "name": "你好",
        "description": "海綿寶寶"
      }
    ]
  },
  "code": "000000",
  "success": true
}
```

### `PATCH` /bapi/mp/v2/management/apps/:appId

Example: `/bapi/mp/v2/management/apps/niHpLXzhjkocTrKvT1T7oQ`

Update app information for the given appId. Now only i18N contents can be applied in this API. Whole app information after this update will be returned to you.

* body
```ts
{
  "i18nContents": Array<{
    "languageCode": string,
    "name": string,
    "description": string
  }>
}
```

* response (same as GET)
```ts
{
  "data": {
    "appId": string,
    "i18nContents": Array<{
      "languageCode": string,
      "name": string,
      "description": string
    }>
  },
  "code": string,
  "success": boolean
}
```

Example for response:
```json
{
  "data": {
    "appId": "niHpLXzhjkocTrKvT1T7oQ",
    "i18nContents": [
      {
        "languageCode": "en",
        "name": "hello",
        "description": "spongebob"
      },
      {
        "languageCode": "zh-TW",
        "name": "你好",
        "description": "海綿寶寶"
      }
    ]
  },
  "code": "000000",
  "success": true
}
```

### `DELETE` /bapi/mp/v2/management/apps/:appId/i18n

Example: `/bapi/mp/v2/management/apps/muRMCXzsh1FmK9dWuHjr3W/i18n?languageCodes=en,zh-TW`

Delete app's i18n information for the given appId and language codes. The remaining i18n info will be returned to caller.

* query
```ts
{
  "languageCodes": string
}
```

Example for query:
```
languageCodes=en,zh-TW
```

* response
```ts
{
  "data": {
    "appId": string,
    "i18nContents": Array<{
      "languageCode": string,
      "name": string,
      "description": string
    }>
  },
  "code": string,
  "success": boolean
}
```

Example for response:
```json
{
  "data": {
    "appId": "niHpLXzhjkocTrKvT1T7oQ",
    "i18nContents": [
      {
        "languageCode": "ja",
        "name": "こんにちは",
        "description": "スポンジボブ"
      },
    ]
  },
  "code": "000000",
  "success": true
}
```
