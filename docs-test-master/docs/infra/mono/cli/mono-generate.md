---
id: mono-generate
title: mono generate
---

Use `generate` command to create project from template.

## Shuvi

Shuvi is a solution for front-end application development.

Example:
```
mono generate -t shuvi
```

### TS Template Directory Structures(Recommend)

```
.
├── .shuvi.config
│   ├── utils
│   │   └── getKeyByTheme.js  // ssr cache
│   ├── csp.js                // define csp in http headers
│   ├── proxy.js              // local api proxy
│   ├── README.md
│   ├── routes.js             // define routes of your app 
│   ├── runtimeConfig.js      // shuvi runtime config
│   └── webpack.js
├── src
│   ├── model                 // rematch config, see https://github.com/rematch/rematch 
│   │   ├── app
│   │   │   ├── model.ts
│   │   │   └── selector.ts
│   │   ├── models.ts
│   │   └── store.ts
│   ├── pages
│   │   └── index.tsx         // home page
│   ├── types
│   │   └── global.d.ts       // define typescript global types
│   ├── utils
│   │   ├── responseInterceptor.ts
│   │   └── index.ts          // define util functions
│   ├── app.electron.tsx      // electron app entry
│   ├── app.ts                // global app entry
│   ├── app.web.tsx           // web app entry
│   ├── constants.ts          // define constant
│   ├── document.ts           // html related configuration, like next.js
│   ├── plugin.ts             // use plugins, such as redux
│   └── server.ts             // ssr configuration
├── .babelrc.js
├── .env                      // environment variables, will not overwrite environment variables in the container, usually used for local development
├── .env.prod                 // no need
├── .env.qa                   // no need
├── .gitignore
├── BUILD.bazel               // define the resources and dependence paths that bazel needs to build
├── deps.bzl                  // define the dependence, if it has been defined in BUILD.bazel, you can delete this file
├── tsconfig.json             // tsconfig
├── MENTIONS
├── OWNERS                    // declares who can review and approve the pr which is relevant to the files beneath this directory
├── package.json
├── README.md
├── shuvi.config.js           // shuvi plugin config 
└── values.yaml               // second configuration of the container, you can define domain names and routes, etc.
```

### JS Template Directory Structures

```
.
├── .shuvi.config
│   ├── utils
│   │   └── getKeyByTheme.js  // ssr cache
│   ├── csp.js                // define csp in http headers
│   ├── proxy.js              // local api proxy
│   ├── README.md
│   ├── routes.js             // define routes of your app 
│   ├── runtimeConfig.js      // shuvi runtime config
│   └── webpack.js
├── src
│   ├── model                 // rematch config, see https://github.com/rematch/rematch 
│   │   ├── app
│   │   │   ├── model.js
│   │   │   └── selector.js
│   │   ├── models.js
│   │   └── store.js
│   ├── pages
│   │   └── index.jsx         // home page
│   ├── app.electron.jsx      // electron app entry
│   ├── app.js                // global app entry
│   ├── app.web.jsx           // web app entry
│   ├── constants.js          // define constant
│   ├── document.js           // html related configuration, like next.js
│   ├── plugin.js             // use plugins, such as redux
│   └── server.js             // ssr configuration
├── .babelrc.js
├── .env                      // environment variables, will not overwrite environment variables in the container, usually used for local development
├── .env.prod                 // no need
├── .env.qa                   // no need
├── .gitignore
├── BUILD.bazel               // define the resources and dependence paths that bazel needs to build
├── deps.bzl                  // define the dependence, if it has been defined in BUILD.bazel, you can delete this file
├── jsconfig.json
├── MENTIONS
├── OWNERS                    // declares who can review and approve the pr which is relevant to the files beneath this directory
├── package.json
├── README.md
├── shuvi.config.js           // shuvi plugin config 
└── values.yaml               // second configuration of the container, you can define domain names and routes, etc.
```

## Lib

Create lib for other applications to use

Example:
```
mono generate -t lib
```

### TS Template Directory Structures(Recommend)

```
.
├── src
│   ├── index.ts              // app entry
│   └── utils.ts              // define util method
├── BUILD.bazel               // define the resources and dependence paths that bazel needs to build
├── deps.bzl                  // define the dependence, if it has been defined in BUILD.bazel, you can delete this file
├── jest.config.js            // jest unit test tool config 
├── package.json
├── README.md
├── tsconfig.build.json       // ts config used by build
└── tsconfig.json
```

### JS Template Directory Structures

```
.
├── src
│   └── index.ts              // app entry
├── BUILD.bazel               // define the resources and dependence paths that bazel needs to build
├── deps.bzl                  // define the dependence, if it has been defined in BUILD.bazel, you can delete this file
├── jest.config.js            // jest unit test tool config 
├── package.json
└── README.md
```

## CICD

Configure cicd for your application to automate integrated and deployment.

There are four environment configurations of dev/ qa1/prod-hk/prod-com can be created, you can choose according to your needs.

Example:
```
mono generate -t cicd
```

> [CICD Management](https://cicd-manage-fe.toolsfdg.net) will help you do this more easily.


### Dev Env Template

```
.
├── infra
    └── k8s-resources
        └── dev
            └── web-ui
                └── ${appName}
                    └── ${appName}.cm.yaml
├── web
└── ...
```

infra/k8s-resources/dev/web-ui/\${appName}/\${appName}.cm.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ${appName}
  namespace: web-ui
data:
  SENTRY_DSN: ""
# add your dev environment here, like
# REACT_APP_NAME: "NAME"
```

### Qa1 Env Template

```
.
├── infra
    └── k8s-resources
        └── qa1
            └── web-ui
                └── ${appName}
                    └── ${appName}.cm.yaml
├── web
└── ...
```

infra/k8s-resources/qa1/web-ui/\${appName}/\${appName}.cm.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ${appName}
  namespace: web-ui
data:
  SENTRY_DSN: ""
# add your qa1 environment here, like
# REACT_APP_NAME: "NAME"
```

### Prod-hk Env Template

```
.
├── infra
    └── k8s-resources
        └── prod-hk
            └── prod
                └── ${appName}
                    ├── ${appName}.cm.yaml
                    └── OWNERS
├── web
└── ...
```

infra/k8s-resources/prod-hk/prod/\${appName}/\${appName}.cm.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ${appName}
  namespace: web-ui
data:
  SENTRY_DSN: ""
# add your prod-hk environment here, like
# REACT_APP_NAME: "NAME"
```

infra/k8s-resources/prod-hk/prod/\${appName}/OWNERS

```yaml
# See the OWNERS docs at https://go.k8s.io/owners

labels:
- area/${appName}
approvers:
- yolin-huang
- com-qa
- fiat-qa
- risk-qa
- wallet-qa
# approvers means that you can input the `/approve` label in github pr to allow your pr to be merged
# add what you want in `mono/OWNERS_ALIASES`

reviewers:
- yolin-huang
- com-qa
- fiat-qa
- risk-qa
- wallet-qa
# compared with `approvers`, `reviewers` has more permissions
# add what you want in `mono/OWNERS_ALIASES`

```

### Prod-com Env Template

```
.
├── infra
    └── k8s-resources
        └── prod-com
            └── prod
                └── ${appName}
                    ├── ${appName}-gray
                    │   └── kustomization.yaml
                    ├── ${appName}-prod
                    │   └── kustomization.yaml
                    ├── ${appName}.cm.yaml
                    ├── ${appName}.svc.yaml
                    └── OWNERS
├── web
└── ...
```

infra/k8s-resources/prod-com/prod/\${appName}/\${appName}.cm.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ${appName}
  namespace: web-ui
data:
  SENTRY_DSN: ""
# add your prod-com environment here, like
# REACT_APP_NAME: "NAME"
```

infra/k8s-resources/prod-com/prod/\${appName}/\${appName}.svc.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: ${appName}
  name: ${appName}
  namespace: prod
spec:
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: http
  selector:
    app: ${appName}
  type: ClusterIP
```

infra/k8s-resources/prod-hk/prod/\${appName}/OWNERS

```yaml
# See the OWNERS docs at https://go.k8s.io/owners

labels:
- area/${appName}
approvers:
- yolin-huang
- com-qa
- fiat-qa
- risk-qa
- wallet-qa
# approvers means that you can input the `/approve` label in github pr to allow your pr to be merged
# add what you want in `mono/OWNERS_ALIASES`

reviewers:
- yolin-huang
- com-qa
- fiat-qa
- risk-qa
- wallet-qa
# compared with `approvers`, `reviewers` has more permissions
# add what you want in `mono/OWNERS_ALIASES`
```

infra/k8s-resources/prod-hk/prod/\${appName}/\${appName}-gray/kustomization.yaml

```yaml
namespace: prod
helmCharts:
  - releaseName: ${appName}-gray
    version: 1.3.9
    repo: https://public.bnbstatic.com/fe-helm/charts
    name: web-ui
    valuesInline:
      service:
        enabled: false
      istio:
        enabled: false
        subsetName: gray
      canary:
        enabled: false
      github:
        repoOwner: mono
        repoName: mono
      nameOverride: ${appName}
      image:
        repository: 918323888678.dkr.ecr.ap-northeast-1.amazonaws.com/fe/${appName}
        tag: ${tag}
        # bot will automatically update this field, like
        # https://git.toolsfdg.net/mono/mono/pull/53462/files
      env:
        - name: BUILD_ENV
          value: production
      envFrom:
      - configMapRef:
          name: common-ui
      - configMapRef:
          name: common-ui-gray
      - configMapRef:
          name: ${appName}
    # can add other configurations, like
    # - configMapRef:
    #     name: ${appName}
      containerPort: ${port}
      # container listen port, usually is `3000`
      health: /health-check
      resources:
        requests:
          cpu: 10m
          # machine performance configuration
      trafficPolicy:
        connectionPool:
          http:
            http1MaxPendingRequests: 20
            # how long does the http request time out
          tcp:
            maxConnections: 20
            # max qps
```

infra/k8s-resources/prod-hk/prod/\${appName}/\${appName}-prod/kustomization.yaml
```yaml
namespace: prod
helmCharts:
  - releaseName: ${appName}-prod
    version: 1.3.9
    repo: https://public.bnbstatic.com/fe-helm/charts
    name: web-ui
    valuesInline:
      service:
        enabled: false
      istio:
        enabled: false
        subsetName: prod
      canary:
        enabled: false
      github:
        repoOwner: mono
        repoName: mono
      nameOverride: ${appName}
      image:
        repository: 918323888678.dkr.ecr.ap-northeast-1.amazonaws.com/fe/${appName}
        tag: ${tag}
        # bot will automatically update this field, like
        # https://git.toolsfdg.net/mono/mono/pull/53462/files
      env:
        - name: BUILD_ENV
          value: production
      envFrom:
      - configMapRef:
          name: common-ui
      - configMapRef:
          name: ${appName}
    # can add other configurations, like
    # - configMapRef:
    #     name: ${appName}
      containerPort: ${port}
      # container listen port, usually is `3000`
      health: /health-check
      resources:
        requests:
          cpu: 100m
          memory: 400m
          # machine performance configuration
      hpa:
        enabled: true
        minReplicas: 2
      trafficPolicy:
        connectionPool:
          http:
            http1MaxPendingRequests: 200
            # how long does the http request time out
          tcp:
            maxConnections: 200
            # max qps
```
