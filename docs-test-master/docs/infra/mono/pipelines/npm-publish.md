---
id: npm-publish
title: lib Publish to Npm
---

## Getting started

How to publish a lib

There are two releases, one is the intranet server and the other is the npm public server

## How it work

version in package.json

```json
"version": "0.0.8"
```

you can update this version by running `mono version --patch` ,It will update the version number of the package whose content has been modified

Modify the version in package.json of lib and merge to master, CICD will publish the lib to the internal server.

If you want to publish the package to public npm server 
add the following code to your package.json

```json
"publishConfig": {
    "public": true,
}
```
