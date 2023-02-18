---
id: mono-add
title: mono add
---

Use `add` command to add npm packeage to the mono sub packages.

Example:

```
mono add -p react@16.10.0 --path libs/api --dev
```

The process is as follows:
1. Search all packages and list all versions that are used in mono
2. Choose to use the latest version available or the version number you specify
3. Add package in the package.json 

## `-p`(required)


Specify the package and verions you want. example:`mono add -p react@16.10.0 --path libs/api`. 
If the version number is not specified，example: `mono add -p react --path libs/api`
We will use the latest version number in mono
If there is no such package in mono, we will use the latest version on npm
Example:
```
mono add -p react --path libs/api
mono add -p react@16.10.0 --path libs/api
```

## `--path`(required)

Set the path of the package you specify 
Example:
```
mono add -p react --path libs/api
```

## `--dev`(optional)

Set this we will add this package to `devDependencies` field

Example:
```
mono add -p react --path libs/api --dev
```

## `--peer`(optional)

Example:
```
mono add -p react --path libs/api --peer
```

Set this we will add this package to `peerDependencies` field

If there is no `--peer` and `--dev` we will save package into `dependencies` field

Example:
```
mono add -p react --path libs/api
```


