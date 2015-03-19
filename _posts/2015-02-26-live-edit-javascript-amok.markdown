---
layout: post
title: Live Edit JavaScript with Amok
excerpt: Our development workflow is completely broken, It would be much better if we could just edit our application live while its running, and have it continue running after editing without having to reload it nor losing its state.

categories:
  - javascript
  - tools
redirect_from:
  - /javascript/tools/live-editing-javascript-amok/
---
Think about how many times we have to reload when we are developing our JavaScript applications. Quite a lot, not to mention the number of times we do it a second time because we weren't sure if we had done it right the first time around since browsers have two or three different modes of refreshing. So once we have our application reloaded, we then have to click and move ourselves around to work our application back into the state we were testing previously, It might not seem like much, but this constant edit, reload and test cycle is incredibly tedious and time consuming.

Our development workflow is basically completely broken, a scripting language we shouldn't have to have this edit, compile and test cycle. It would be much better if we could just edit our application live while its running, and have it continue running after making edits to it without having to reload or restart the application, keeping its state intact.

## Introducing Amok

[Amok](https://github.com/caspervonb/amok) is an editor agnostic, standalone tool that gives us the ability to do live editing of our application while it's running without ever having to reload or restart the application. It works by connecting to the browser where our application is already running through a debugging session, so there are no browser side plugins to install, we just have to enable debugging in the target browser by providing it with some flags when launching it.

Since the code is never re-evaluated but re-compiled instead, [side effects](http://en.wikipedia.org/wiki/Side_effect_%28computer_science%29) that would be executed from running the script top to bottom will not be executed, think of it as the existing code being hot patched to the new definitions. In a traditional compiler/debugger toolset this sort of feature would typically be called *edit and continue*.

Amok can be installed [via npm](https://www.npmjs.com/package/amok)

```sh
npm install amok -g
```

## Running Our Application

Running our application is straightforward, to start our application we just pass the entry point scripts along to amok, amok will then watch these files and re-compile them when the files change.

```sh
amok canvas.js
```

See the video for a short demonstration

<div class="embed-container">
<iframe src="https://www.youtube.com/embed/xHXqyfkct2w?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>
</div>

## Configuring the Browser 

The browser needs to be launched with remote debugging enabled, this is to allow amok to establish a debugging session to it.

To make amok spawn chrome by default, we can set an environment variable called `BROWSER` to the command we use to launch the browser.

```sh
BROWSER="BROWSER COMMAND"
```

### Chrome

For Google Chrome, the remote debugging port has to be specified through the `remote-debugging-port` option, the default expected value is 9222. So we would simply call it from the command line like the following:

```sh
google-chrome --remote-debugging-port=9222
```

## Configuring the Bundler
We can also, optionally use a bundler, or other preprocessor to compile our application. Amok will use the command defined in a environment variable called `BUNDLER` to process the source files it knows about.

If the processor has a watch mode, for optimal performance it is recommended to use this as it will do incremental builds and caching, as opposed to doing a full rebuild every time a source file changes.

## Browserify

```
export BUNDLER="watchify"
```

## Babel

```
export BUNDLER="babel --watch"
```

Any command line options after the options stop delimiter `--` will be passed through to the bundler. For example to use watchify with reactify

```
export BUNDLER = "watchify"
amok app.js -- --transform reactify
```

<div class='embed-container'>
<iframe src="https://www.youtube.com/embed/-aWINzxCNW4?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>
</div>

## Differences with Livereload

Livereload does a pretty good job at dealing with web sites, but not applications. Despite popular misconception it does not actually do live editing, it does live reloading, it's essentially like having someone sitting behind you hitting the refresh button for you.

However, you may still want to keep livereload around and run it in cooperation with amok. Because currently amok does not deal with CSS or HTML, so you may still want to stick to livereload for applying HTML and CSS changes, basically the can augment each other for a better experience.

See the video for a short side by side comparison:

<div class='embed-container'>
<iframe src="https://www.youtube.com/embed/RcOFZ_zZOmU?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>
</div>


---

[![Support the fundraiser](https://cloud.githubusercontent.com/assets/157787/6731116/4a310ec6-ce7d-11e4-9866-1332f739da4b.png)](https://www.bountysource.com/fundraisers/682-amok-live-editing-javascript)
---
