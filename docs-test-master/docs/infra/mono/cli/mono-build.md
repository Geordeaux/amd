---
id: mono-build
title: mono build
---


Use `build` command to build the mono application locally.

It will do the following things:
1. Analyze which libs are needed
2. Build these libs
3. Build the application

## `-p`(required)

### basic

The mono application is in the mono/web/apps directory.

Example:

```
mono build -p home-ui
```

### extra

You can also use other commands to combine.

Example:
```
mono build -p home-ui -e
mono build -p home-ui -c 'yarn build:dev'
```

## `-e`(optional)

The default build method is `bazel tsc`. Its advantage is to make local build products the same as CI.

Adding this parameter to use `script esbuild` to build. This make the local build more fast.

Example:
```
mono build -p home-ui -e
```

## `-c`(optional)

The default start command is `yarn build`. You can overwrite it.

Example:
```
mono build -p home-ui -c 'yarn build:dev'
```
