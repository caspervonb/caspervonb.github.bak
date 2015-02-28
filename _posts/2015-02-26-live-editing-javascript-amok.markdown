---
layout: post
title: Live Editing JavaScript with Amok
categories: javascript tools
---

## Introduction

Think about how many times you reload your application when you are developing
your JavaScript application. Quite a lot, I guess, not to mention the number of
times you do it a second time because you weren't sure if you had done it in the
first place, then you have to get your application back into the state you were
testing in previously, It might not seem like much, but this edit, reload, test
cycle gets really tedious and time consuming.

It would be much better if we could just edit our application live while its
running and continue running after editing without having to reload nor loosing its state.

Luckily, there's a brand new tool for doing just that called Amok.

<iframe width="800" height="450" src="https://www.youtube.com/embed/xHXqyfkct2w?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

The neat thing about Amok is that it connects to the client through a debugging session, so there are no special hacks or tricks. It's just how the runtime works, that being the case it can work with any kind of javascript application, including react.

<iframe width="800" height="450" src="https://www.youtube.com/embed/-aWINzxCNW4?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

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

## Configuring Your Browser to Work with Amok

Amok works out of the box, but your browser
however does need a little configuration to coooperate with Amok,
it has to be launched with remote debugging enabled.

> Currently, Chrome is the only supported client.
> Need support for Firefox or another client?
> Consider [tipping](https://www.gittip.com/caspervonb) to vastly speed up development.

### Chrome
For Google Chrome, the remote debugging port has to be specified,
one can do this by appending `--remote-debugging-port=<PORT>` when launching chrome.

If you want Amok to spawn the client for you, set the `BROWSER` environment variable to a value which will make Amok spawn it pointing at the server host and port after the server has started listening.

For easier usage, you can store this value in your user shell resource file.

```
BROWSER="google-chrome --remote-debugging-port=9222"
```

## Configuring Your Bundler to Work with Amok
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

Any command line options after the options stop delimeter `--` will be passed through to the bundler.
For example to invoke watchify with coffeeify

```
export BUNDLER="watchify"
amok app.js -- -t coffeeify
```

## Comparing with Other Tools
So how does Amok differ from other tools out there at the moment?

### LiveReload

<iframe width="800" height="450" src="https://www.youtube.com/embed/RcOFZ_zZOmU?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

LiveReload does a pretty good job at dealing with web sites, but not applications.
Despite popular misconception it cannot do live editing, only live reloading, it's essentially like having someone sitting behind you hitting the refresh button for you.

However, you may still want to keep LiveReload around and run it in cooperation with Amok. Because currently Amok does not deal with CSS or HTML, so you may still want to stick to LiveReload for applying HTML and CSS changes.
