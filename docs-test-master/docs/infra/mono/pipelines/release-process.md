---
id: release-process
title: Release Process
---

This article describes how to release in mono. Please make sure that you have prepared the `environment variables` and `prowjob configuration` to deploy prod. If not, please see the [here](https://docs.fe.devfdg.net/docs/infra/mono/best-practices/mono-generate#cicd) for how to solve it.

Here are the steps to release the application:

## Dev Or Qa1

1. Trigger a postsubmits prowjob, this means that merge the application code into master branch. like [this](https://git.toolsfdg.net/mono/mono/pull/56044/files#diff-15abcec31b6e506bacd64961cf7f4957R2685):
![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/postjob.png)
2. Wait the job finish, see build detail [here](https://prow.toolsfdg.net/view/s3/toolsnew-fe-prow-logs/logs/post-publish-mono-live-web-ui/1454031458027966464).
3. The dev or qa1 environment will be created automatically after a few minutes. Open [kiali dev](https://kiali.devfdg.net/kiali/console/workloads?namespaces=web-ui&opLabel=or)/[kiali qa1](https://kiali.qa1fdg.net/kiali/console/workloads?namespaces=web-ui) for more details.
4. Open your dev or qa1 website to verify if is succeed.
5. Use `/publish ${ appName}` to rebuild the dev and qa1 environment in pr if you have new commit.

## Prod

1. When your code is merged into the master branch, prowjob will build app for you. The code will be deployed to the dev qa1 environment. If your set `--disable_dev` or `--disable_qa1` in prowjob, the corresponding environment will not be deployed.


2. The `bot` will help you create a `gray` pr again. 
You can find it from [here](https://git.toolsfdg.net/mono/mono/pulls), like this:
![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/gray-pr.jpg)



3. You can use this pr to cooperate with the chrome plugin [FeHeader](https://chrome.google.com/webstore/detail/feheader/omloppmdelcfeablljmjmonacajpfpmj) and use the `k8scluster:true` as `${key}:${value}`. Then come to gray website to verify your application. 
4. If the test passes, ask someone with permission to comment on `/lgtm` in the `gray` pr, and then it can be integrated and launched. like this:
![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/bot-release-gray.png)

5. Then `bot` will create a `release` pr again. After ensuring that there is no problem with the previous operation, will be deployed after comment `/lgtm`.
like this:
![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/prod-pr.jpg)
