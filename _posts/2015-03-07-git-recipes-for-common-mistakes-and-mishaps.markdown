---
layout: post
title: Git Recipes for Common Mistakes and Mishaps
categories: 
  - tools
related_books:
  - 
    title: Version Control with Git
    thumbnail: https://images-na.ssl-images-amazon.com/images/I/51n%2BRSsEmEL._SL160_.jpg
    excerpt: "Get up to speed on Git for tracking, branching, merging, and managing code revisions. Through a series of step-by-step tutorials, this practical guide takes you quickly from Git fundamentals to advanced techniques, and provides friendly yet rigorous advice for navigating the many functions of this open source version control system."
    url: http://amzn.to/1KzEgF5
    
  - 
    title: Pro Git
    thumbnail: https://images-na.ssl-images-amazon.com/images/I/51yUrziP2fL._SL160_.jpg
    excerpt: Pro Git (Second Edition) is your fully-updated guide to Git and its usage in the modern world. Git has come a long way since it was first developed by Linus Torvalds for Linux kernel development. It has taken the open source world by storm since its inception in 2005, and this book teaches you how to use it like a pro.
    url: http://amzn.to/1MeXNGg

redirect_from:
  - /tools/git-recipies-for-the-rest-of-us/
  - /tools/git-recipes-for-the-rest-of-us/
  - tools/git-recipes-for-the-common-mistakes-and-mishaps/
---

> Got a recipe that should have been here?
> Leave a comment below.

In this day and age, nearly everyone uses git. From designers to engineers we all depend on it. Despite git being a really good version control system, enevitably we do sometimes mess up and have to fix our configuration and/or working tree, or just plain forget how to do that one thing we rarely do.

## Change authentication timeout
Git can remember your https credentials if you aren't using a ssh key, integrates nicely with the native keychain.
The default timeout is rather short however, this can be changed via

```sh
git config --global credential.helper "cache --timeout=<SECONDS>"
```

For a full work day, this could be set to 28800.

## Change Message of the Previous Commit
To fix a typo, or other error in the previous commit before pushing to remote,
we can simply add --append to the git commit command and we will get the editor again.

```sh
git commit --amend
```

## Add a File to the Previous Commit
To add a file that we forgot to add in the previous commit, we can simply add it
via `git add <filename>`, then call `git commit -amend` to add the changeset to the latest commit.

```sh
git add <filename>
git commit --amend
```

## Undo Changes to a File before a Commit

```sh
git reset <filename>
```

## Undo the previous commit
To undo the most recent commit we can run `git reset HEAD~1`.
If we pass `--soft` option the working tree will be kept intact letting us
use `git add` and friends to do the commit again.

```sh
git reset --soft HEAD~1
```

To remove the commit completely, resetting the changed files back to their
original state we can pass the `--hard` option.

```
git reset --hard HEAD~1
```

## Clean the Working Tree
To remove files that are not being tracked, we can use `git clean`.

```sh
git clean --force
```

## Change Author of the Previous Commit
To fix the author after, for example forgetting to set `user.name` and
`user.email`. We can run `git commit` with the `--reset-author` option
after having updated the configuration fields.

```sh
git config user.name "Your name"
git config user.email you@example.com

git commit --amend --reset-author
```

## Change Anything Else
To fix anything else from squashing to re-ordering commits,
git rebase with the `--interactive` option is the way forward.

```sh
git rebase --interactive
```

## Change Author Details of All Their Own Commits
To fix an author's name and/or email in all their commits,
we can run the following little shell snippet.

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
