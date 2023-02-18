---
id: publish-document
title: How to Publish a Document
hide_title: true
---

# How to Publish a Document

## Introduction

For sharing document to Binance members, here is the website [https://docs.devfdg.net/](https://docs.devfdg.net/) for Web (e.g. Infra, i18n, Trade and C2C) and Native (Android and iOS) developers.

In this article, I will introduce that how to publish a document on the website with a Pull Request (PR) from GitHub.

## Getting Started

### Create a Fork Repository

The GitHub repository we used to link the documents is [https://git.toolsfdg.net/mono/mono](https://git.toolsfdg.net/mono/mono). Instead of creating a branch on it and creating a pull request from this repository, we need to "fork" this repository.

![Fork button](https://static.devfdg.net/static/mono-static/docs-ui/img/_2021-01-08_10.03.34.png)

It's easy to fork this repository by clicking the "Fork" button. After clicking that button, there should be an private repository with the same repository name "mono" under your workspace:

![create private repo](https://static.devfdg.net/static/mono-static/docs-ui/img/_2021-01-08_10.06.17.png)

And you could check there is a text under your private "mono" repository: "forked from mono/mono" which means that you have an "snapshot" of the original repository and you could do anything to your private repository without affecting the original one.

### Clone the Private Repository

After forking the original repository, you should have a forked "mono" repository now. If not, go to the previous step and check again.

Clone your private repository to local and add a remote repository URL:

![Clone your private repo](https://static.devfdg.net/static/mono-static/docs-ui/img/_2021-01-08_10.15.19.png)

```shell
# Replace "<your_github_id>" with your GitHub ID
git clone https://git.toolsfdg.net/<your_github_id>/mono.git
cd mono
git remote add upstream https://git.toolsfdg.net/mono/mono.git
git remote -v

# The output should be like this:
# origin	https://git.toolsfdg.net/<your_github_id>/mono.git (fetch)
# origin	https://git.toolsfdg.net/<your_github_id>/mono.git (push)
# upstream	https://git.toolsfdg.net/mono/mono.git (fetch)
# upstream	https://git.toolsfdg.net/mono/mono.git (push)
```

### Run and Test

1. Install [Yarn](https://classic.yarnpkg.com/en/) which is a package management tool. Please follow the instruction: https://classic.yarnpkg.com/en/docs/install/

2. Initialize the `mono` CLI, which will help to start, build, or even generate an app:

```shell
bash ./tools/scripts/init.sh
# if you are using bash
source ~/.bash_profile
# if you are using zsh
# source ~/.zshrc

mono -h

# start docs-ui
mono start -p docs-ui
```

and you can check the result with the URL [http://localhost:3000/](http://localhost:3000/) in your browser.

### Create a Feature Branch

Before creating a document you'd like to share, create a feature branch in your local private repository.

```shell
# Replace "<branch_name>" with the branch name you want
git checkout -b <branch_name>

# pull and reset to the latest code from upstream
git pull upstream master && git reset --hard upstream/master
```

### Create a Document

#### The document format: Markdown

To share a document on [https://docs.devfdg.net/](https://docs.devfdg.net/), we should create a Markdown format file with the `.md` or `.markdown` extension. If you are not familiar with Markdown format, please refer to the page: [https://v2.docusaurus.io/docs/markdown-features/](https://v2.docusaurus.io/docs/markdown-features/).

#### The document path

To add a document under iOS ([https://docs.devfdg.net/docs/native/ios/overview](https://docs.devfdg.net/docs/native/ios/overview)), please follow the architecture under the path: "mono/web/apps/docs-ui/docs/native/ios/".

If you would like to add images in your document, please put your image files under "./assets/" folder next to your markdown file. And wrap the "./assets" folder and the markdown file in the folder named as your file ID. For example:

```shell
mono/web/apps/docs-ui/docs/native/ios/publish-document/
├── assets
│   ├── _2021-01-08_10.03.34.png
│   ├── _2021-01-08_10.06.17.png
│   └── _2021-01-08_10.15.19.png
└── how-to-publish-a-document.md
```

### Update the Sidebar

For organizing the sidebar of the document, you could group files and rename the group. Please refer to the [document](https://v2.docusaurus.io/docs/sidebar) for more detail information.

For example:
```json {4-11} title="mono/web/apps/docs-ui/config.json"
  Android: {
    Infra: ['native/android/overview'],
  },
  iOS: {
    'Getting Started': [
        'native/ios/overview',
        'native/ios/publish-document/publish-document',
        'native/ios/Team POC/team-poc'
    ],
    'Legacy Documents': ['native/ios/legacy-ios-documentations']
  },
  '101': {
    'Getting Started': ['101/overview'],
    'Thinking & Culture': ['101/culture/ground-rules', '101/culture/tech-culture'],
```

![sidebar demo](https://static.devfdg.net/static/mono-static/docs-ui/img/_2021-01-08_4.11.52.png)

### Create a Pull Request

After finished your local changes and verified in your local webpage (http://localhost:3000/), please commit and push it to your remote GitHub workspace.

```shell
git add -p .
git commit

git remote get-url origin
# https://git.toolsfdg.net/<your-workspace>/mono.git

git push origin <your-branch-name>
```

and go back to your workspace on GitHub and switch to your branch you just pushed.

![switch branch on GitHub](https://static.devfdg.net/static/mono-static/docs-ui/img/_2021-01-08_4.31.13.png)

Click the "Pull request" button:

![Pull request button](https://static.devfdg.net/static/mono-static/docs-ui/img/_2021-01-08_4.31.39.png)

and remember to check the selected base repository should be "mono/mono" and the base branch should be "master":

![_2021-01-08_4.37.58](https://static.devfdg.net/static/mono-static/docs-ui/img/_2021-01-08_4.37.58.png)

Post the pull request link to the reviewers listed in the file "OWNERS" in Webex channel "sig-docs" and wait for approval.

### Congrate!

After the pull request approved and merged, you can check your changes on the website https://docs.devfdg.net/.

## Edit an Existed Document

Scroll down to the page bottom and find the "Edit this page" botton:

![Edit this page botton](https://static.devfdg.net/static/mono-static/docs-ui/img/2021-01-14_9.21.46.png)

Click this botton and be redirect to the edit mode of that page on GitHub:

![Edit mode](https://static.devfdg.net/static/mono-static/docs-ui/img/2021-01-149.31.44.png)

> You’re editing a file in a project you don’t have write access to. Submitting a change to this file will write it to a new branch in your fork your-workspace/mono, so you can send a pull request.

After editing this page and ready to propose, scroll down to the bottom and fulfill the proposal of this change:

![Propose file change](https://static.devfdg.net/static/mono-static/docs-ui/img/2021-01-14_9.38.37.png)

Just like the pull request in previous section, the change will on the website https://docs.devfdg.net/ after the PR merged.

## Review Update on a Test Server

Except reviewing your updates in the local mono server, there is another way to check the updates: on a test server.

1. Please following the [instruction](https://docs.devfdg.net/docs/infra/feheader/instructions) to install a Chrome extension: FEHeader.
2. Add a comment with `/publish docs-ui` in the pull request.
   ![2021-01-22-5.53.36](https://static.devfdg.net/static/mono-static/docs-ui/img/2021-01-22-5.53.36.png)
3. Wait for the task "pull-mono-docs-ui-publish" completed
   ![2021-01-25-10.19.00](https://static.devfdg.net/static/mono-static/docs-ui/img/2021-01-25-10.19.00.png)
4. Set-up the request header to FEHeader with the key "docs-ui" and the value "pr-{pull-request-id}".
   ![2021-01-22-5.54.27](https://static.devfdg.net/static/mono-static/docs-ui/img/2021-01-22-5.54.27.png)
5. Check your update on the test server: https://docs.fe.devfdg.net/

## Reference

* Document website: https://docs.devfdg.net/
* Markdown features: https://v2.docusaurus.io/docs/markdown-features
* Mono repository: https://git.toolsfdg.net/mono/mono
* Test website: https://docs.fe.devfdg.net/
* FEHeader installation instructions: https://docs.devfdg.net/docs/infra/feheader/instructions
