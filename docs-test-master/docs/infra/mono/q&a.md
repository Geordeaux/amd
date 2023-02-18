---
id: q&a
title: Q&A
---

> If you encounter problems in mono, you can try to find the answer here.

## Q: During the deployment phase, an error of `serve` appears

![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/yarn-server-error.jpg)

Why: The system did not find the `serve` command in the package.json file. 

> Two situations are known.


How:
- It is a spa service abd using nginx and no `yarn serve` command is required, so please specify dockerfile as spa in prowjob, such as `--path_to_dockerfile apps/shared/Dockerfile.spa` or the path of your custom dockerfile path like `--path_to_dockerfile apps/fe-ops-ui/Dockerfile`.
- You do need to add a `yarn serve` command to start your service.

## Q: aws s3 cp --no-progress s3://*** fatal error: An error occurred(404) when calling the HeadObject operation: Key does not exist
![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/static-error.png)
Why: You specified that you need to upload static resources to s3, but you did not find the resources that need to be uploaded to s3.

> Two situations are known.

How: 
- The default path for code compilation and upload to s3 is from `dist/client/static` to `static`. But the `build path` of your app may change, you can re-specify the path of the resource, such as `--static "build => uc/v3"`.
- You do not need to upload static resources to s3, then add `--disable_uploading_statics` to prowjob.

## Q: job `pull-mono-if-lockfile-changed` error
![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/yarn-lock-error.png)

Why: You did not commit the `yarn.lock` file in your application to the remote.

How: commit it.

## Q: I have a build problem with `/opt/bin/build-mono: line 45: mono: command not found.`

Why: There is no clone code, so no mono is found.

How: Change `fast-publish_template` in job to `publish_template`.

## Q: Module not found: Can't resolve `styled-components` in '/mono/apps/legacy/exchange-user-center-ui/components'

Why: Styled-components adds `alias` reference, and styled-components is installed in the root directory node_modules.

How: Delete alias.

## Q: I do `mono generate`, but some error occurred when creating a project

Why: The package is not installed correctly.

How: 
```
rm -fr node_modules && rm -fr apps/**/node_modules && rm -fr libs/*/node_modules
```
