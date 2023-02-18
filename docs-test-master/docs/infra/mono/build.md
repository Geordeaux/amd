---
id: build
title: Build
---

This document introduces the principle of `mono build`.

## 1. bazel query

We use Bazel to build libs. Bazel query libs that needs to be build.

Example:
```bash
mono build -p template-ui

# tools/bin/mono-build

# This command will find all needed libs.
deps=$(bazel query "kind(pkg_npm,siblings(deps(//apps/${app}:build) intersect //libs/...))")

# Below are all needed libs
# You can find it in web/apps/${appName}/BUILD.bazel or web/apps/${appName}/deps.bzl  `deps` field
```

![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/build-libs.png)

## 2. build libs

Bazel then build needed libs.

```bash
# This command will build needed libs.
bazel build ${deps}
```

![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/bazel-build-process.png)

After do this, lib dist build by Bazel will link to every lib.


## 3. build app

We use  `yarn build` or your custom commands to build the app. Below is succeed message.

![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/mono-build-yarn-build.png)
