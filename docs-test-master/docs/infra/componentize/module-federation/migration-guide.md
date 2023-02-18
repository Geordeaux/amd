---
id: migration-guide
title: Migration guide
---

### Step 1: Declare expose components

```diff
// apps/mainuc-support-ui/shuvi.config.js
{
+ asyncEntry: true,
  plugins: [
    // ... others plugins
+   [
+     '@binance/shuvi-plugin-module-federation',
+     {
+       name: 'support',
+       exposes: {
+         './FeedbackEntry': './pages/feedback-entry',
+       },
+       shared: require('@binance/mainuc-shared/build.config/sharedConfig'),
+     },
+   ],
  ]    
}
```

### Step 2: Consume components in shell

```javascript
// apps/mainuc-shell-ui/src/routes/contexts/support.ts
export const routes = [
  {
    module: 'support', // should matching with remote package's name
    ns: [I18N_NS_USER_SUPPORT],
    layout: false,
    routes: [
      {
        exact: true,
        path: `/user-support/feedback/entry`,
        component: loadComponent('support', './FeedbackEntry'),
      },
    ],
  },
]

```

### Step 3: Add corresponding URLs to shell 

```diff
// web/apps/mainuc-shell-ui/values.yaml
containerPort: 80
envFrom:
  - configMapRef:
      name: common-ui
  - configMapRef:
      name: mainuc-shared-ui
  - configMapRef:
      name: mainuc-shell-ui
istio:
  hosts:
    - www.devfdg.net
    - www.qa1fdg.net
  match:
+   - uri:
+       regex: '^/[a-zA-Z-]+/my/user-support/feedback(/.*)?$'
labels:
  cache-proxy: "true"
```

```diff
// web/apps/mainuc-support-ui/values.yaml, remove old uri
containerPort: 80
envFrom:
  - configMapRef:
      name: common-ui
  - configMapRef:
      name: mainuc-shared-ui
istio:
  hosts:
    - www.devfdg.net
    - www.qa1fdg.net
  match:
-   - uri:
-       regex: "^/[a-zA-Z-]+/my/user-support/feedback(/.*)?$"
+   - uri:
+       regex: "^/en/mainuc-support-ui/version$"
labels:
  cache-proxy: "true"
```

### Step 4: Update prowjobs

```diff
// infra/prow/jobs/prowjobs-pr-publish-mono.yaml
  - name: pull-publish-mono-mainuc-support-ui
    spec:
      containers:
      - <<: *container_spec
        command:
        - /bin/prow
        - |
          publish-mono \
            --app_name mainuc-support-ui \
            --static "build => uc/v3" \
+           --static "build => static/permanent/mainuc-support-ui" \
-           --path_to_dockerfile apps/shared/Dockerfile.spa \
+           --path_to_dockerfile apps/shared/Dockerfile.spa-mf \
          
  - name: post-publish-mono-mainuc-support-ui
    spec:
      containers:
      - <<: *container_spec
        command:
        - /bin/prow
        - |
          publish-mono \
            --app_name mainuc-support-ui \
            --static "build => uc/v3" \
+           --static "build => static/permanent/mainuc-support-ui" \
-           --path_to_dockerfile apps/shared/Dockerfile.spa \
+           --path_to_dockerfile apps/shared/Dockerfile.spa-mf \
```

## Local development:
#### Option 1:
1. Using a static server to serve build file from remote apps
2. Build apps and copy build result to static server directory
3. Point remote server config to appropriate URL 
```markdown
// mainuc-shell-ui/.env
REMOTE_URL_BROWSER=http://remote-server...
```
4. Start mainuc-shell-ui

## Example usage for static server
Source code: [Simple server](https://git.toolsfdg.net/duc-ho/simple-server)
For instance, we want to examine compatibility `mainuc-dashboard-ui` within `mainuc-shell-ui`
- First we need to build `mainuc-dashboard-ui` by: `mono build -p mainuc-dashboard-ui`
- Then copy the build file from dashboard-ui to static server
  - Copy `mono/web/apps/mainuc-dashboard-ui/build/static`
  - Paste into `simple-server/src/static/permanent/mainuc-dashboard-ui`
- Start `simple-server` on port 3001 by `yarn dev`
- Finally, we can consume dashboard's components from remote server by starting `mono start -p mainuc-shell-ui`, 
don't forget to point remote URL to simple server's port by modify `.env` file by following `REMOTE_URL_BROWSER=http://localhost:3001/static/permanent`

#### Option 2:
Develop your apps as usual, then check the compatibility on Dev or QA environment. Please make sure that you changed appropriate uri in values.yaml

## Troubleshooting

### 1. Context provider and consumer are not singleton

Error: ```Cannot read property 'isMobile' of undefined``. Because the provider and the consumer do not point to same reference.

Fix:
```diff
- import { useMedia } from '@shared/utils/media'
+ import { useMedia } from '@binance/mainuc-shared'
```

### 2. Add shared library/module

If the library is used in more than 1 remote apps, add this to ```web/libs/mainuc-shared/build.config/sharedConfig.js```

