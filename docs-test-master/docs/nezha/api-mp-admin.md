---
id: api-mp-admin
title: Api for Admin
sidebar_label: Admin  
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

1. Browser redirect to OKTA Oauth
2. OAuth redirect back to the `redirect_uri`
3. Browser handle the callback from OAuth and send code from OAuth to browser.
4. Browser send a `GET` mp-api/v1/management/sessions?code=[insert code here]
5. Backend will save a cookies called `session` in browser.

## Session

###  `GET` /bapi/mp/v1/admin/sessions

Example: /bapi/mp/v1/admin/sessions?code=test123test

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

###  `DELETE` /bapi/mp/v1/admin/sessions

delete session

* response
```ts
boolean
```

## User 
### `GET` /bapi/mp/v1/admin/user/info

get user info

* response

```ts
{
  "email": string,
  "role": {
    "name": string,
    "permissions": Array [
      "own",
      "edit",
      "view",
    ],
  },
  "userId": number,
}
```

## Members

### `GET` /bapi/mp/v1/admin/members

get admin user list

* response

```ts
Array<{
  createdAt: number,
  updatedAt: number,
  id: number,
  name: string,
  email: string,
  scope: string,
  permissions: string,
}
}>
```

### `POST` /bapi/mp/v1/admin/members

invite/update new or existing user that can be admin or auditor 

* body

```ts
{
  email: string,
  roleId: number
}
```

* response (same as GET)

```ts
Array<{
  createdAt: number,
  updatedAt: number,
  id: number,
  name: string,
  email: string,
  scope: string,
  permissions: string,
}
}>
```

### `DELETE` /bapi/mp/v1/admin/members

remove user

* body

```ts
{
  userId: number,
}
```

* response (same as GET)

```ts
Array<{
  createdAt: number,
  updatedAt: number,
  id: number,
  name: string,
  email: string,
  scope: string,
  permissions: string,
}
}>
```

## Role

### `GET` /bapi/mp/v1/admin/roles

get roles available

* response

```ts
Array<{
  "createdAt": number,
  "id": number,
  "name": string,
  "permissions": string,
  "scope": string,
  "updatedAt": number,
}>
```

## Audit

### `GET` /bapi/mp/v1/admin/audits

* params

```ts
{
  page: number,
  status: 'todo' | 'approved' | 'rejected' | 'all'
}
```

* response

```ts
Array<{
  "applicationVersion": {
    "createdAt": number,
    "description": string,
    "downloadUrl": string,
    "id": number,
    "isExperience": boolean,
    "publishedTimestamp": number | null,
    "revision": string,
    "sdkVersion": string,
    "status": "DEVELOP",
    "updatedAt": number,
    "version": string,
  },
  "createdAt": number,
  "id": number,
  "isApproved": boolean | null,
  "remark": string | null,
  "updatedAt": number,
}>
```

* metadata

```ts
{
  "currentPage": number,
  "itemCount": number,
  "itemsPerPage": number,
  "totalItems": number,
  "totalPages": number,
}
```

### `PUT` /bapi/mp/v1/admin/audits

* body

```ts
{
  approve: boolean,
  versionId: number
}
```

* response

```ts
boolean
```

