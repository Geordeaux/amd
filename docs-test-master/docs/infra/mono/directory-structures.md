---
id: directory-structures
title: Directory Structures
---

## Directory Structures

```
.
├── infra
│   ├── dashboard             // grafana dashboard jsons
│   ├── experiment            // experiment scripts
│   ├── helm
│   │   └── charts            // stores all the internal helm charts
│   ├── images                // stores all the internal docker images which will be upload to AWS ECR
│   ├── k8s-resources         // stores all the k8s resources which can be apply directly
│   │   ├── dev               // internal dev cluster(for developers)
│   │   ├── general           // will be apply to dev, qa1, prod-com and prod-hk
│   │   ├── prod-com          // production cluster(for users not in China)
│   │   ├── prod-hk           // production cluster(for users in China)
│   │   ├── qa1               // internal qa1 cluster(for qa)
│   │   ├── tools             // internal tools cluster(e.g: prow, apphost, tekton)
│   ├── label_sync            // labels used by prow to show on github
│   ├── prow                  // stores all prow configs(config, plugin, prowjob)
│   ├── statics
│   │   └── prod              // stores the version of production statics
│   └── Makefile              // contains some useful scripts
├── protos                    // protocol buffers' definitions
│   ├── backend
│   └── schema
├── tools
│   ├── bin                   // executable scripts(e.g: mono)
│   ├── scripts               // scripts used by mono
│   ├── generators            // yeoman generators
│   └── OWNERS
├── resources
│   └── i18n                  // legacy! it should be replaced by I18n CMS(BTS)! all web i18n translations
│       ├── cloud-json
│       ├── pexpay
│       ├── social-trading
│       └── web
├── web
│   ├── apps                  // all business applications
│   │   ├── *-ui
│   │   ├── legacy            // legacy apps use independent yarn.lock
│   │   └── shared            // used by all applications
│   ├── libs                  // all the web libraries will be stored in this directory
│   ├── tools
│   │   ├── bazel-patchs      // bazel patches
│   │   ├── rules             // custom bazel rules
│   │   └── scripts           // custom scripts
│   ├── .bazelignore
│   ├── .bazelrc              // bazel runtime config
│   ├── .bazelversion
│   ├── .editorconfig
│   ├── .eslintignore
│   ├── .eslintrc.js
│   ├── .gitignore
│   ├── .prettierignore
│   ├── .prettierrc
│   ├── .yarnrc.yml
│   ├── BUILD.bazel           // contains bazel rules
│   ├── README.md
│   ├── WORKSPACE             // bazel workspace file
│   ├── jest.config.base.js
│   ├── jest.config.js
│   ├── package.json
│   ├── setupTests.ts
│   ├── tsconfig.build.json
│   ├── tsconfig.jest.json
│   ├── tsconfig.json
│   └── yarn.lock
├── .dockerignore             // docker ignore, used by kaniko
├── .yamllint
├── OWNERS                    // declares who can review and approve the pr which is relevant to the files beneath this directory
└── OWNERS_ALIASES            // declares groups aliases of owners
```


