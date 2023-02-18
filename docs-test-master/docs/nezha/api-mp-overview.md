---
id: api-mp-overview
title: MP-Management API Overview
sidebar_label: Overview
---

> tip: there are multiple apps under one account, and one app contains multiple versions.

## Response

All responses have a uniform structure just like our gateway-api

```ts
interface Response {
  code?: string;
  message?: string;
  messageDetail?: string;
  data?: Object | Array;
  metadata?: Object;
  success: boolean;
}
```

If `success` is true, you'll only get the `data` field, otherwise you'll get the `code`, `message` and `messageDetail` fields.

In the followed api doc, we will only specify the content in the `data` field.

