---
layout: post
title: "Programming Style Should Not Be a Choice"
categories:
  - rant
---

We often tell our peers, especially juniors that programming style is just a matter of personal preference, and i used to share this sentiment, but the more i think about it the more i realize that this is just bad advice, even tho the compiler might allow you to write braces or spaces in five different styles, that doesn't mean you should. The language, in combination with the standard library always sets a precedence.

## C/C++
C and C++ are prime examples of this, in their early years the standard libraries were more or less non existant and so every company had their own standards, heck for the longest time people stayed away from using the standard template library and would rather chew off their own leg before having to use it. So as a consumer of these libraries you would end up juggling seven different types that represented strings, and every single one of these parts all used their own coding conventions from hungarian notation to unix styles, should a function be in lowercase, uppercase, camelcase, pascal case or underscore seperated? Just spin a wheel, because the code was so noisy anyways it diddn't matter.

It's only recently that people have been starting to uniformly try to follow the style and conventions of the standard library, but even now there is still the question of what is the file extension of a header file? `.hh`, `.hpp`, `.hxx`, `.hcc` or just `.h`, because you'll get to use all of them every time you include your dependencies.

There's quite alot more to the story of both C and C++ but it stands as an example as to how too much freedom lead to fragmentation between libraries, making interopability difficult, and just being noisy in general, the language is fine, the libraries are not, so jumping in to a C++ codebase is generally like russian roulette, you have no idea what you are going to get, in the case of C, its a much simpler language so the odds are more favorable of a codebase being decent.

## Ruby
Ruby on the other hand came in with conventions tied to the language itself, for example anything starting with a capital letter is a constant, so classes would be capitalized and value constants would be in upper case, It's a very simple concept but very powerful as it sets an unbreakable precedence as for how to write code. This coupled with the ruby *hipsterness* lead to very coherent and uniform practices. Basically the code will look about the same regardless of what library you use, and sort of know what you can expect when maintaining an existing codebase.

## JavaScript
JavaScript is both kind of old and kind of new, obviously the web isn't exactly new but the library ecosystem surrounding node is only a couple of years old. So it has the advantage of being able to cherry pick practices from everywhere this story is almost the same as with ruby, except JavaScript gives you a loaded shotgun you can shoot yourself in the foot with, it's called automatic semi colon insertion. Seasoned developers know of it, and some choose to have as few semi colons as possible in their code, then a less seasoned developer comes around and has no idea why their small edit broke the code, it looks just fine, but of course the linting rules that would catch it are disabled for the project, an example of developers choosing the more error prone approach to express their superiority.

## Conclusion
Don't try to be clever, code is meant to be readable not an artistic form of expression. Your unique way of indentation or braces doesn't make you the next picasso, it just pisses off the next developer in line to maintain the project. If there is a precedence in the language or community then don't go out of your way to scratch your own itch. Just follow the damn conventions, if there is one the community has failed.
