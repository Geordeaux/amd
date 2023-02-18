---
id: yaml-validator
title: yaml-validator
---
`yaml-validator` is an external Prow plugin for validating and linting YAML files. It runs either when
1. a PR is opened, reopened or changes the state; or  
2. when a user adds a `/validate-yaml` comment on an open PR.

Currently, this plugin peform following actions:
- prow job config validation.
- check for overlapping URI regex of VirtualService.
- lints YAML files.

The plugin config is defined in [infra/prow/plugins.yaml](https://git.toolsfdg.net/mono/mono/blob/master/infra/prow/plugins.yaml) file.


### How to invoke manually ?
Add a `/validate-yaml` comment on a open PR
