---
layout: post
title: Live Editing JavaScript with Amok
categories: javascript tools
---

<iframe width="100%" height="500px" src="https://www.youtube.com/embed/xHXqyfkct2w" frameborder="0" allowfullscreen></iframe>

## Introduction

Think about how many times you reload your application when you are developing
your JavaScript application. Quite a lot, I guess, not to mention the number of
times you do it a second time because you weren't sure if you had done it in the
first place, then you have to get your application back into the state you were
testing in previously, It might not seem like much, but this edit, reload, test
cycle gets really tedious and time consuming.

It would be much better if we could just edit our application live while its
running and continue running after editing without having to reload nor loosing its state.

## Getting Amok

Amok is a command line tool that serves your application, allowing you to edit
your code in *any editor*, and have the changes automatically sourced and
recompiled in the connected client without having to reload,
keeping your application state intact.

Amok is installable through [npm](npm).

```
npm install amok -g
```

## Running your application with Amok
Running your applications with Amok is straight forward, zero configuration approach,
it should **just work** out of the box. Just pass in your entry points as arguments to the command.

```
amok mydep.js myapp.js
```

## Configuring your browser to work with Amok

Amok works out of the box, but your browser
however does need a little configuration to coooperate with Amok,
it has to be launched with remote debugging enabled.

> Currently, Chrome is the only supported client.
> Need support for Firefox or another client?
> Consider [tipping](https://www.gittip.com/caspervonb) to vastly speed up development.

### Chrome
For Google Chrome, the remote debugging port has to be specified,
one can do this by appending `--remote-debugging-port=<PORT>` when launching chrome.

If you want Amok to spawn the client for you, the `BROWSER` environment variable to a value which will make Amok spawn
it pointing at the server host and port after the server has started listening.

For easier usage, you can store this value in your user shell resource file.

```
BROWSER="google-chrome --remote-debugging-port=9222"
```

## Configuring your bundler to work with Amok
Amok works with a variety of bundlers, just set the `BUNDLER` environment
variable and Amok will use it to process your bundle.

## Browserify
```
BUNDLER="watchify"
```

## Babel
```
BUNDLER="babel --watch"
```
