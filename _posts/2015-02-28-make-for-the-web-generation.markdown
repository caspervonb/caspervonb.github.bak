---
layout: post
title: Make for the Web Generation
categories: javascript tools
---

## Introduction
JavaScript has provided us with a giant explosion of build tools, [grunt][grunt], [gulp][gulp], [slush][slush], [broccoli][broccoli] and [brunch][brunch] just to name a few of the more popular ones, there is just way to many to name them all.

More or less these tools all depend on plugins to do even the simplest of tasks, from copying a file to creating an archive, you'll need a plugin for it.

While, in theory this leads to great flexibility, in reality we are just duplicating the unix ecosystem. This also quickly leads to your project having a big bundle of development dependencies, and the tasks you are doing are just run-of-the-mill ordinary copying, building, bundling and minification.

## Introducing Make
Make is a pretty old tool, and for most developers coming from a native development background it is an old friend, but most web developers that don't come from a computer science background don't really seem to ever get introduced to it. However, that does not mean *make* is not a good tool, in-fact its far too underrated these days.

You have the entire unix ecosystem available to you, pipes, streams, utilities, it's all there, most of the tools you'll need to build are already on the system. If you are developing on a non-unix machine like Windows, then you should at least do yourself a favor and install a proper shell and a minimal bundle of tools like [gow][gow], perhaps a better terminal emulator to replace `cmd.exe` as-well, like [cmder][cmder].

Make works by defining rules in a file typically called `Makefile`, no file extension, and do take note that capitalization matters. Rules are made up from targets and prerequisites, which can have shell commands executed upon them, which is usually referred to as the recipe.

In general a rule looks something like this:
```make
targets: prerequisites
    recipe
# Do take note, that is a tab, not spaces.
```

So with that knowledge, lets do a simple example. We want to copy a file from `input.txt`which is the prerequisite to `output.txt`which is the target. Our recipe will be a simple command line utility.

```make
output.txt: input.txt
  cp $< $@
```

Pretty simple right? This does a shell invocation of [cp(1)](cp), with a couple of [automatic variables][make-automatic-variables], `$@` holds the file name of the target, `$<` holds the name of the first prerequisite.

Lets move on to something more practical.

## Compiling Your Code
Coffeescript, Typescript, or modern JavaScript transpilers like [babel][babel] are pretty common nowadays, so we'll compile our code from modern JavaScript to something that can be consumed today with [babel][babel] as our first example.

To do this we will need to define a rule that does a one to one conversion between the files in our *src* directory, to the transpiled files we want in our *lib* directory.

```make
# First we will assign a variable to our JavaScript compiler,
# babel in this case. While this is not strictly nesscescary it
# makes maintenance a little easier.
JC            = babel

# We'll enable loose transformations
JCFLAGS       = --loose

# Next we'll find all the source files in our source directory
SRC           = shell(find src -name "*.js")

# And then do substitution to get strings that map one to one from
# the source directory to the library directory
LIB           = $(SRC:src/%.js=lib/%.js)

# Finally, we'll define our rule to convert from source to library
# Which is to invoke the compiler with the options and prerequiresite,
# then output to the target.
$(LIB): $(SRC)
  $(JC) $(JCFLAGS) $< -o $@
```

## Bundling Your Code
Browserify, Webpack or just plain concatenation is also a rather standard thing to do if you are going to distribute your code for the browser environment. In order to do this we will need a rule that has a single target, takes all the sources files as prerequisites, feeding them through the bundler as the recipe.

```make
# Like before, we will store our bundler in a variable
BUNDLE        = browserify

# Along with its invocation flags.
# This is an example of where using variables makes things easier
# We will want to pass the same flags to both babel and
# the babelify transform, since it is already a 
# variable there is no duplication.
BUNDLEFLAGS   = --transform babelify [$(JCFLAGS)]

# Next we'll define our bundle filename
DIST          = dist.js

# Finally we'll define the rule, again the goal is to pass all
# prerequisites into the bundler and output it to the target.
# We could have used the already compiled library files,
# but generally this would yield a slightly bigger file
# since compilers often generate helpers.
$(DIST): $(SRC)
  $(BUNDLE) $(JCFLAGS) -o $@ $^
```

## Optimizing Your Code
Minification and dead code removal is another common build step. In this case we'll just define a rule that takes the previously defined bundle target as a prerequisite and run it through uglify, we'll do some simple substitution to get our minified bundle name based on the non-minified bundle name.

```make
# We will use uglify as our optimizer
OPT           = uglify

# No flags, just yet atleast.
OPTFLAGS      = 

# This rule will be really simple.
# It's a single target with a single prerequisite passed through
# our optimizer, we will however do some simple substitution to
# get a target has a "min" suffix.
$(DIST:%.js=$(DIST).min.js): $(DIST)
  $(OPT) OPTFLAGS -o $< $@
```

## Conclusion
Make has a bit of a undeserved reputation for being hard to understand, it is not that different from shell scripting. In my opinion the syntax is clear and concise, you get free interop with everything, it has streaming, it has pipes, it has a big ecosystem called *unix*, so with that said. **Viva la revolución, write a makefile today!**

Feel free to hate on make at me via [@caspervonb on Twitter](http://twitter.com/caspervonb)

[grunt]: http://gruntjs.com/ "Grunt"
[gulp]: http://gulpjs.com/ "Gulp"
[slush]: http://slushjs.github.io/#/ "Slush"
[broccoli]: https://github.com/broccolijs/broccoli "Broccoli"
[brunch]: http://brunch.io/ "Brunch"

[coffeescript]:http://coffeescript.org "CoffeeScript"
[typescript]: http://www.typescriptlang.org "TypeScript"
[babel]: https://babeljs.io "Babel"
[gow]:  https://github.com/bmatzelle/gow/wiki "GNU on Windows"
[cmder]:http://bliker.github.io/cmder/ "Portable console emulator for Windows"

[cp]: http://linux.die.net/man/1/cp "copy files and directories"
[make-automatic-variables]: https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html "Make: Automatic Variables"
