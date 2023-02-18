---
id: mono-start
title: mono start
---


Use `start` command to start the mono application locally.

It will do the following things:
1. Analyze which libs are needed
2. Build these libs
3. Build and run the application

## `-p`(required)

### basic

The mono application is in the mono/web/apps directory.

Example:

```
mono start -p home-ui
```

### extra

You can also use other commands to combine.

Example:
```
mono start -p home-ui -w uikit-widget
mono start -bp home-ui
mono start -p home-ui -c 'yarn start:dev'
```

## `-w`(optional)

The mono lib is in the mono/web/libs directory.

Add the `-w libName` parameter to watch the changes of the local library.

Also, you can add multiple `-w` parameters to watch multiple lib

Example:
```
mono start -p home-ui -w uikit-widget

mono start -p home-ui -w uikit-widget -w api-sdk
# multiple lib
```
## `-b`(optional)

The default build method is `script esbuild` it not cache the build result.

Adding this parameter to run esbuild with bazel cache, i.e. bazel will use cached the result for unchagned package instead of build it again.

Example:
```
mono start -bp home-ui
```

## `-t`(optional)

The default builder esbuild does not generate type definition for typescript code, If your app required type definition please use this flag.

Adding this parameter to use `bazel typescript` to build. This make the local build products the same as CI.

Example:
```
mono start -p home-ui -t
```

## `-c`(optional)

The default start command is `yarn start`. You can overwrite it.

Example:
```
mono start -p home-ui -c 'yarn start:dev'
```
