---
published: false
layout: post
title: Git Recipes for the Rest of Us
categories: 
  - tools
related_books: 
  - title: Version Control with Git
    excerpt: "Get up to speed on Git for tracking, branching, merging, and managing code revisions. Through a series of step-by-step tutorials, this practical guide takes you quickly from Git fundamentals to advanced techniques, and provides friendly yet rigorous advice for navigating the many functions of this open source version control system."
    url: "http://amzn.to/1KzEgF5"
---

In this day and age, nearly everyone uses git from designers to engineers, and having a good git flow is key, but enevitably we do sometimes mess up and need to clean up our mess before merging or pushing our source tree.

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