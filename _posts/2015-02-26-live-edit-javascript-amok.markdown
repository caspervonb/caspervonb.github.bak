---
layout: post
title: Live Edit JavaScript with Amok
categories: javascript tools
redirect_from:
  - /javascript/tools/live-editing-javascript-amok/
---

## Introduction

Think about how many times we have to reload when we are developing our JavaScript applications. Quite a lot, not to mention the number of times we do it a second time because we weren't sure if we had done it right the first time around since browsers have two or three different modes of refreshing. So once we have our application reloaded, we then have to work our application back into the state we were testing previously, It might not seem like much, but this constant edit, reload and test cycle is really tedious and time consuming.

Our development workflow is completely broken, It would be much better if we could just edit our application live while its running, and have it continue running after editing without having to reload it nor losing its state.

## Introducing Amok
[Amok][amok] is a rather new command line utility that gives us the ability to do live editing of our application, it works by connecting to the browser where our application is already running through a debugging session, so there are no plugins to install, we just have to enable debugging in the target browser by providing it with some flags.

Since the code is never re-evaluated but re-compiled instead, [side effects][wikipedia-side-effects] that would be executed from running the script top to bottom will not be executed, think of it as the existing code being hot patched to the new definitions. In a traditional toolset this sort of feature would typically be called *edit and continue*.

To get amok, we need to install it via npm:
```sh
npm install amok -g
```

## Running Our Application

Running our application is straight forward, 

```sh
amok canvas_demo.js
```

See the video for a short demonstration:
<iframe width="800" height="450" src="https://www.youtube.com/embed/xHXqyfkct2w?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>


## Configuring the Browser 

The browser needs to be launched with debugging enabled in order to allow amok to connect to it, how we do this varies a bit depending on the browser.

> Currently, Chrome is the only supported client. Need support for Firefox or another client?
>  [Tips allow me to dedicate more time working on amok](https://www.gittip.com/caspervonb)

### Chrome

For Google Chrome, the remote debugging port has to be specified through the `remote-debugging-port` option, so we would simply call it from the command line like the following:

```sh
google-chrome --remote-debugging-port=9222
```

To make amok spawn the browser for us by default, we can set an environment variable called `BROWSER` to the command we use to launch the browser.

```sh
BROWSER="google-chrome --remote-debugging-port=9222"
```

And if we want this to always happen by default, we can write this to our shell resource file.

```sh
echo 'cat BROWSER="google-chrome --remote-debugging-port=9222"' >> ~/.bashrc 
```

## Using a Bundler
We can also use a bundler for compiling our application,  amok will use the command defined in a environment variable called `BUNDLER` to process our application sources.

## Browserify
```
export BUNDLER="watchify"
```

## Babel
```
export BUNDLER="babel --watch"
```

Any command line options after the options stop delimeter `--` will be passed through to the bundler. For example to use watchify with reactify

```
export BUNDLER = "watchify"
amok app.js -- --transform reactify
```

<iframe width="800" height="450" src="https://www.youtube.com/embed/-aWINzxCNW4?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

## Comparisons
> If you feel any of the comparisons are biased, please ping me via [@caspervonb on Twitter](http://twitter.com/caspervonb)

How does amok compare to other tools?

### Livereload

Livereload does a pretty good job at dealing with web sites, but not applications. Despite popular misconception it does not actually do live editing, it does live reloading, it's essentially like having someone sitting behind you hitting the refresh button for you.

However, you may still want to keep livereload around and run it in cooperation with amok. Because currently amok does not deal with CSS or HTML, so you may still want to stick to livereload for applying HTML and CSS changes.

See the video for a short side by side comparison:
<iframe width="800" height="450" src="https://www.youtube.com/embed/RcOFZ_zZOmU?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

[amok]: https://github.com/caspervonb/amok/ "Amok"
[wikipedia-side-effects]: http://en.wikipedia.org/wiki/Side_effect_%28computer_science%29 