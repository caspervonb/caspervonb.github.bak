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
first place, then you have to get your application back into the state you were in previously,
It might not seem like much but this edit, reload, test cycle gets really tedious and time consuming.
It would be much better if we could just edit our application live while its
running and continue after editing without having to reload nor loosing its state.

## Getting Amok

Amok is a command line tool that serves your application and
allows you to edit your code in *any editor*, and have the changes automatically
reloaded and recompiled in the connected clients without having to reload the page,
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
Your browser however does need a little configuration,
namely it needs to be started with remote debugging enabled.

### Chrome
For Google Chrome, you can do this by starting it with `--remote-debugging-port=9222`

If you want Amok to spawn the client for you,
you can set the `BROWSER` environment variable to an appropriate value.

```
BROWSER="google-chrome --remote-debugging-port=9222"
```
