---
id: get-start
title: Get Start
---

Here is preparation before starting to code

## Step 1. Get the authority of the github organization and repository(optional)

> If you already have github permission, you can skip this step.

You need `mono` github repository permissions to clone code, get it from your leader.

Or you can open [CICD Management](https://cicd-manage-fe.toolsfdg.net) and follow the steps in the picture.

![snapshot](https://static.devfdg.net/static/mono-static/docs-ui/img/step-of-approval.png)

> After doing the above, your leader will receive a notification from webex, then approve it

## Step 2. Clone `mono` repository

1. Open mono remote [website](https://git.toolsfdg.net/mono/mono.git)
2. Fork repo into your own git project workspace
3. Clone your repo to locale, like `git clone https://git.toolsfdg.net/kami-c/mono.git`
4. Cd into your local mono workspace, like `cd /path/mono`
5. Add remote, like `git remote add upstream https://git.toolsfdg.net/mono/mono.git`
6. Check if remote is ready `git remote -v`

## Step 3. Initialize `mono` command line

Cd your mono workspace path like `cd /path/mono`

```
bash ./tools/scripts/init.sh
# if you are using bash
source ~/.bash_profile

# if you are using zsh
# source ~/.zshrc 

mono -h
# usage: mono <subcommand> [options] [parameters]
# ...
```


