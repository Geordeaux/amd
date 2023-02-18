---
id: mono-version
title: mono version
---

Use `version` command to patch the package's version if package‘s conntent was modified.

Example:

```
mono version --patch
```
 


## `--patch`(required)

Update versions if the package is modified
The workspace should be clean

And it will do a commit to submit the version changes

Example:
```
mono version --patch
```