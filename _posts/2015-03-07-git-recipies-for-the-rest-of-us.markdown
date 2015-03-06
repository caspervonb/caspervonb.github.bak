---
layout: post
title: Git Recipes for the Rest of Us
categories:
  - tools
---

> Preview, This article is under development

## Change Message of the Previous Commit

```sh
git commit --amend
```

## Undo Changes to a File before a Commit

```sh
git reset <filename>
```

## Undo the previous commit

```sh
git reset --hard HEAD~1
```
 
## Change Author of the Previous Commit

```sh
git config user.name "Your name"
git config user.email you@example.com

git commit --amend --reset-author
```

## Change Author of All Commits

```sh
#!/bin/sh

git filter-branch --env-filter '
OLD_EMAIL="old-email@example.com"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="your-correct-email@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```
