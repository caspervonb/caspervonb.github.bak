---
layout: post
title: Makefile Recipes For Node and JavaScript Projects
categories:
  - javascript
  - tools
---

When you write JavaScript code for either Node or the Web, you certainly will have a couple of things in your build test and release cycle to automate, lets take a quick look at how to do some of the more common ones with a Makefile

## Compile with CoffeeScript

```make
SRC         = $(shell find src -name "*.coffee")
LIB         = $(SRC:src/%.coffee=lib/%.js)

$(LIB): $(SRC)
  coffee $< -o $@
```

## Compile with TypeScript

```make
SRC         = $(shell find src -name "*.coffee")
LIB         = $(SRC:src/%.coffee=lib/%.js)

$(LIB): $(SRC)
  tsc $< -o $@
```

## Compile with Babel

```make
SRC         = $(shell find src -name "*.coffee")
LIB         = $(SRC:src/%.coffee=lib/%.js)

$(LIB): $(SRC)
  babel $< -o $@
```
