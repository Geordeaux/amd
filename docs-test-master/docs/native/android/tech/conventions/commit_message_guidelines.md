---
id: commit_message_guidelines
title: Commit Message Guidelines
hide_title: true
---

# Commit Message Guidelines

## Brief description of current situation

As we all know, in our daily development process, we use Git to submit a large amount of code every day, and each submission needs to write a commit message (commitment description). The standardized Commit message has many benefits:
- You can browse the git commit history tree conveniently, intuitively and quickly, and go back to the previous development content for easy review.
- The change log can be generated directly from the canonical commit, which is used to illustrate the version difference at the time of release.

At present, we do not have the corresponding specification for git commit message, which may cause the following troubles:
- Each developer has a different style, and the format is messy and not uniform, making it inconvenient to view.
- Part of the commit did not fill in the message, it is difficult to locate the bug modification history in the follow-up review, or the specific change point of a feature, because the commit is so messy, we don't know which commit should correspond to which business module.

## Brief description of current situation

Simple is efficient. In order to facilitate the use and avoid overly complicated regulations, we use the following submission format:

formation:
[type][scope][version] : subject

example:
[feat][Fiat/share][1.36.0]: share function in fiat version 1.36.0

### 1. Type

type is the category of commit. Currently, only the following commonly used types are listed, and you are welcome to add positively.

- fix: fix bug
- feat: new feature(feature)
- perf: Various types of performance optimization modifications
- refactor: Refactoring type modification  
- style : Code format change
- docs: documentation (documentation) addition or change
- test: add test case related
- build: Changes to build tools, build process, and compilation scripts
- lang: multilingual internationalization

### 2. Scope

We use scope to describe the scope of our changes to facilitate the location and traceability of follow-up issues.
The scope can be a component name, a module name, or a business name, followed by a more granular function name. For example: Fita/share, it is recommended to use English uniformly here.

### 3. Version

Corresponding to the version number of the submitted commit.

### 4. Subject

The subject is a brief description of this submission. Try to be as simple and clear as possible. If it is a bug fix, it is recommended to bring the bug fix number.
It is recommended to start with a verb, such as: set, modify, repair, add, delete, cancel, etc.
No restriction on Chinese and English.

## Final

Everyone is welcome to supplement and improve this Guidelines.
Attach a comparison chart of good and bad Git commits:

![img](https://confluence.toolsfdg.net/download/attachments/33096367/good-commit.png?version=1&modificationDate=1583936020000&api=v2)

![img](https://confluence.toolsfdg.net/download/attachments/33096367/bad-commit.png?version=1&modificationDate=1583936049000&api=v2)

### Contacts

- [seal.w@binance.com](mailto:seal.w@binance.com)

[Reference](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit)