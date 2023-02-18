---
title: FE Header
---

## Overview

- **What**: add extra headers to all requests before sending to server
- **Why**: we need to view the result published from different environment/Projects/PRs
- **When**: test on dev/qa/gray
- **Who**: anyone would like to test on dev/qa/gray
- **Where** to get it: https://chrome.google.com/webstore/detail/feheader/omloppmdelcfeablljmjmonacajpfpmj 

## How to migrate ModHeader to FEHeader

1. Export from **ModHeader**  
![export_from_ModHeader](https://static.devfdg.net/static/mono-static/docs-ui/img/export_from_ModHeader.png)

2. Copy JSON
```json
[
  {
    "appendMode": false,
    "backgroundColor": "#1e57a1",
    "filters": [],
    "headers": [
      {
        "comment": "",
        "enabled": true,
        "name": "",
        "value": ""
      }
    ],
    "hideComment": true,
    "respHeaders": [],
    "shortTitle": "1",
    "textColor": "#ffffff",
    "title": "Profile 1",
    "urlReplacements": []
  }
]
```

3. Import to **FEHeader**  
![import](https://static.devfdg.net/static/mono-static/docs-ui/img/import.png)

## Headers to use for different environment

### Dev environment

```bash title="add these headers when testing on DEV environment with a specific PR"
x-gray-env: dev
k8scluster: {mono-app-name}-pr-{pull request number}
 
# example
x-gray-env: dev
k8scluster: pay-ui-pr-42494
```

### QA environment

```bash title="add these headers when testing on QA environment with a specific PR"
x-gray-env: qa
k8scluster: {mono-app-name}-pr-{pull request number}
 
# --- example
x-gray-env: qa
k8scluster: pay-ui-pr-42494
```

### Gray environment

```bash title="don't forget x-gray-env header"
# same headers for all projects by default
x-gray-env: gray
k8scluster: true
```

> Notes: by default all projects should use **k8scluster: true** for gray env, but where is true value come from, it's below:

```yaml title="pay-ui.vs.template.yaml"
headers:
    k8scluster:
        exact: 'true' # it comes from here
```

### Production environment - No headers needed

List of possible production domains:

- [https://www.binance.com/](https://www.binance.com/)
- [https://www.binancezh.io/](https://www.binancezh.io/)


Thanks for reading! 🙂
