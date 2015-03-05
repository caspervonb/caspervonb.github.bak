---
layout: post
title: Compile Modern JavaScript with Babel
excerpt: Babel (formerly known as 6to5) is a rather new a source to source compiler that aims to compile the latest, and next versions of the JavaScript language down to code that can run on the runtimes of today, meaning we can just write with modern syntax today without having to worry about runtime support, and as time passes we'll get native runtime support for these features across the board and we can simply stop transpiling that specific feature once it's universally supported.
categories:
  - javacript
  - tools
---

> This article is under development

## Introduction
[The JavaScript specification is evolving at a rapid speed][overview-of-javascript-2015], historically some of the browsers have been rather notoriously bad at keeping up with the specifications, to be fair others have been more at the cutting edge, but waiting for the runtimes to catch up before 

So generally we as developers have been stuck in the era of developing without proper tooling, and we've had to settle for the common denominator between the major runtimes at that time.

In the past couple of years however, we've seen a rise in source to [source compilers (also known as transpilers)](wikipedia-transpilers) to address this issue, but we have mostly been using approach this to invent new languages such as CoffeeScript, TypeScript rather than focusing our efforts on the emerging standards.

## Introducing Babel
[Babel][babel] (formerly known as 6to5) is a rather new a source to source compiler that aims to compile the latest, and next versions of the JavaScript language down to code that can run on the runtimes of today, meaning we can just write with modern syntax today without having to worry about runtime support, and as time passes we'll get native runtime support for these features across the board and we can simply stop transpiling that specific feature once it's universally supported.

Babel is [available][babel-npm] through [npm][npm], and can be installed with

```bash
npm install -g babel
```

## Compiling our code with Babel
Babel is distributed with a command line tool, which works [great with make][make-for-the-web-generation], otherwise there is also a very extensive collection of plugins available for almost every build system imaginable.

[make-for-the-web-generation]: /javascript/make-for-the-web-generation/
[overview-of-javascript-2015]: /javascript/overview-javascript-2015/
[babel]: http://babeljs.io
[babel-npm]: http://babeljs.io
[npm]: https://npmjs.org
