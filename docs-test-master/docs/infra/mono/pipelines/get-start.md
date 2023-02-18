---
id: get-start
title: Get Start
---

This document introduces how to deploy application.

## Generate App

Click [here](https://docs.fe.devfdg.net/docs/infra/mono/cli/mono-generate#shuvi) to learn how to create app.

## Create CICD Configuration

- Open [Cicd Management](https://cicd-manage-fe.toolsfdg.net/cicd/config/create/mono) to create CICD configuration. Please don't skip that website's guide.
- After successful creation, `webex` will notify you, call reviewer to approve this pull request.

## Do Publish

- Change your code and manually create a new pr.
- Comment in pr `/publish ${appName}`
- Go to the dev or qa1 website to verify

## Do Release

Click [here](https://docs.fe.devfdg.net/docs/infra/mono/pipelines/release-process) to learn how to release app.
