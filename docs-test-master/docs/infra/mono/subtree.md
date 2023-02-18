---
id: subtree
title: Subtree
---

## Getting started

For some reasons, we want to extract the projects in mono and become a new repo.

We use git subtree to implement it

## How it work

Extract the project from mono to a new project.

Then run a timer to continuously submit the latest code in mono to the separated project. 

Make it always the latest code

## Subtree cmd

```bash
git clone "https://${GITHUB_USER}:${GITHUB_PASSWORD}@git.toolsfdg.net/mono/mono.git" && cd mono
git subtree  --prefix=web/apps/subtree/developer-docs-ui pull https://git.toolsfdg.net/fe/developer-docs-ui-my.git master
git subtree  --prefix=web/apps/subtree/developer-docs-ui push https://git.toolsfdg.net/fe/developer-docs-ui-my.git master
```