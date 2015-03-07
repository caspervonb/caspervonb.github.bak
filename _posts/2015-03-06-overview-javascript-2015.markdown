---
layout: post
title: An Overview of JavaScript in 2015 (ECMAScript 6)
categories:
  - javascript
excerpt: JavaScript is evolving, ECMAScript 2015 (previously known as ECMAScript 6) is sixth edition of JavaScript, and is the upcoming version of the ECMAScript standard, this standard previously said to be ratified in late 2013, then again in late 2014 is now targeting ratification in June 2015.

related_books:
  - 
    title: "JavaScript: The Good Parts"
    thumbnail: https://cloud.githubusercontent.com/assets/157787/6539252/9f310fe8-c4aa-11e4-9dcb-8aedbb74bc1a.jpg
    excerpt: Most programming languages contain good and bad parts, but JavaScript has more than its share of the bad, having been developed and released in a hurry before it could be refined. This authoritative book scrapes away these bad features to reveal a subset of JavaScript that's more reliable, readable, and maintainable than the language as a whole—a subset you can use to create truly extensible and efficient code.
    url: http://amzn.to/1aRGnE9

  - 
    title: "You Don't Know JS: ES6 & Beyond"
    thumbnail: https://cloud.githubusercontent.com/assets/157787/6495189/b252357e-c302-11e4-8e5b-057c1df8b79a.jpg
    excerpt: No matter how much experience you have with JavaScript, odds are you don’t fully understand the language. As part of the "You Don’t Know JS" series, this compact guide focuses on the new features that will be available to developers in ECMAScript 6, the newest version of the standard on which JavaScript is built.
    url: http://amzn.to/1M6NW5d
redirect_from:
  - /2014/03/05/ecmascript-6-features-and-tools/
  - /2014/03/05/ecmascript-6-features-and-tools.html
  - /archive/ecmascript6-features-and-tools/
---

## Introduction

JavaScript is evolving, ECMAScript 2015 (previously known as ECMAScript 6) is sixth edition of JavaScript, and is the upcoming version of the ECMAScript standard, this standard previously said to be ratified in late 2013, then again in late 2014 is now targeting ratification in June 2015.

## Language Features

This sixth edition of JavaScript introduces a lot of new language syntax features, it might even be the most feature packed revision to date in terms of language additions, and that is with some of the features scheduled, like comprehensions and rest and spread properties being delayed to the next edition.

These features are also backwards compatible, in the sense that they are mostly syntatic sugar and can be de-sugared to older versions of the language, meaning we can use them right now [by compiling with Babel][compile-modern-javascript-with-babel]

### Arrow Functions

Arrow functions are a function shorthand using the `=>` syntax. However, unlike normal functions however, arrows share the same lexical `this` as their surrounding code.

For example, previously with a normal function we would have to capture the value of `this`

```js
this.message = 'hello world';
var that = this;

process.nextTick(function() {
  console.log(that.message);
});
```

Using an arrow function we can now drop that entirely

```js
process.nextTick(() => {
  console.log(this.message);
});
```

Having a function body for an arrow function is optional, it may also be an expression so we can make that even more concise

```js
process.nextTick(() => console.log(this.message));
```

### Binary and Octal Literals

Binary and Octal literals are new forms of numeric literals added for binary and octal numbers, denoted by `b` and `o` respectively.
```
0b111110111 === 503 // true
0o767 === 503 // true
```

### Block Scoping

Block scoping are new forms of declaration for defining variables scoped to a single block, as opposed to variables declared with `var` which have a function-level scope.

#### Let
We can use  let in-place of var to define block-local variables without having to worry about them clashing with variables defined elsewhere within the same function body.

```
for (var i = 0; i < 3; i++) {
   let j = i * i;
   console.log(j);
}
console.log(j); // => error, j is undefined
```

#### Const
`const` follows the same rules as `let`, except that the value is immutable so we can only assign to it once.

```js
const PI = 3.14159265359;
PI = 0; // => 0
console.log(PI); // => 3.14159265359
```

### Classes
Classes are a concise declarative syntax for writing object oriented prototype patterns with a classical classes approach, the class syntax has support for inheritance, super calls, instance and static properties and constructors.

We would typically have written a class as

```js
var Monster = (function() {
    function Monster(name) {
        Entity.call(this);
        this.name = name;
    }
	
	util.inherits(Monster, Entity);
	
    Object.defineProperty(Monster.prototype, "scariness", {
        get: function () {
            return 'mild';
        },
        enumerable: true,
        configurable: true
    });
	
    Monster.prototype.speak = function () {
        Entity.speak.call(this);
    };
	
    Monster.create = function () {
        return new Monster();
    };
	
    return Monster;
})();
```

We can now rewrite with declarative consise syntax

```js
class Monster extends Entity {
  constructor(name) {
    super();
	this.name = name;
  }

  get scariness() {
	return 'mild';
  }
  
  speak() {
    super.speak();
  }

  static create() {
    return new Monster();
  }
}
```

### Default Parameter Values
Default parameter values allows us to initialize parameters when they were not explicitly provided. Where-as previously we would often check for undefined and assign default values.

```js
function f(x, y) {
  y = y || 42;
  return x + y;
}
```

We can now simply inline that information in the function parameters

```js
function f(x, y=42) {
  return x + y;
}
```

### Destructuring Assignments
Destructuring assignment allows us to assign parts of an object to several variables at once.

So lets say we have a `ball`,  the ball is defined as

```js
var ball = {
  position: [0, 0],
  radius: 20,
  elasticity: 1,
  deflated: false,
};
```

Before we had no choice but to extract each value with its own assignment

```js
var x = ball.position[0];
var y = ball.position[1];
var radius = ball.radius;
var elasticity = ball.elasticity;
var deflated = ball.deflated;
```

With destructuring, we can reduce this down to two lines

```js
var [x, y] = ball.position;
var { radius, elasticity, deflated } = ball;
```

### Iterators
Iterators allow iteration over arbitrary objects, while this by itself is not strictly a language feature, rather a protocol/pattern that is implemented by the core library, it does tie into other language features such as generators and for-for which work with this pattern.

The iterator protocol takes the following form, any object can be an iterator as long as it defines a `next()` method.

Any object can be iterable as long as it defines an iterator method, the named of the method is obtained through `Symbol.iterator`, often denoted with `@@iterator`.

```js
function RangeIterator(min, max) {
   this[Symbol.iterator] = function () {
      var current = min;

      return {
         next: function () {
            current++;
            
            return {
               done: current == max,
               value: current,
            };
         }
      }
   };
}
```

#### For Of
The `for-of` loop allows you to conveniently loop over iterable objects.

Example:

```js
for (let i of new RangeIterator([1, 10])) {
   console.log(i);
}
```

### Generators

Generators make it easy to create iterators. Instead of tracking state yourself and implementing `iterator`, you just use yield (or yield* to yield each element in an iterator).

Example:

```js
function *range(min, max) {
   for (var i = min; i < max; i++) {
      yield i;
   }
}

for (let value of range(0, 100)) {
   console.log(value);
}
```

### Method Definition Shorthand

Method Definition Shorthand is a shorthand syntax for method definitions in object initializers, whereas before we would explicitly state the property name.

```js
var obj = {
  toString: function toString() {
    return 'obj';
  };
};
```

We can now infer that from the function name itself

```js
obj = {
   toString() {
      return 'obj';
   },
};
```

### Modules

Modules provide declarative syntax for module patterns, this syntax feels a lot like CommonJS modules, but it has some minor semantic differences.

Primarily it handles default exports a little differently. In CommonJS the default export is the actual export object itself, where-as with ECMAScript 6 modules the default export is just another named export that is supported by syntax.

```js
export function isEqual(a, b) {
  return a == b;
}

export default function assert(expression) {
  return expression == true;
}
```

Named exports are imported via `{ exportName }`

```js
import { isEqual } from './assert';
```

The default export can be imported just by specifying any identifier

```js
import { isEqual } from './assert';
```

Since the default export is a named export, one could import it that way too.

```js
import { default } from './assert';
```

Imports may also be aliased

```js
import { isEqual as checkEqual } from './assert';
```

Wilcard imports are also available

```js
import * as assert from './assert';
```

Which are mirrored by wildcard exports

```js
export * from './assert';
```

### Property Value Shorthand
Property Value Shorthand is a shorthand syntax object initializers whose property keys are initialized by variables of the same name, whereas before we would have to repeat the property key and variable name.

```js
function f( x, y ) {
  return { x: x, y: y };
}
```

We can now leave out the redundant repetition entirely 

```js
function f( x, y ) {
  return { x, y };
}
```

### Rest Parameters
Rest parameters provides a cleaner way of dealing with variadic functions, that is functions that take a arbitrary number of parameters.

```js
function add(...values) {
   let sum = 0;

   for (var val of values) {
      sum += val;
   }

   return sum;
}

add(2, 5, 3); // => 10
```

### Spread Operator
The spread operator allows an expression to be expanded in places where multiple arguments or multiple elements are expected.

```js
var a = [0, 1, 2];
var b = [3, 4, 5];

a.push(...b); // => [0, 1, 2, 3, 4, 5]
```

### Template Strings

Template strings are a new form of string literals using backticks, they are multiline and support interpolation through the `${}` syntax.

```js
console.log(`Hello World,
Today is ${new Date()}
`);
```

## Core Library
The core library has also gotten a bunch of additions,  these include but are not limited to promises, proxies, sets, weakmaps, weaksets and typed arrays to name a few.

Existing objects have also been extended, and core objects are now subclassable.

[Check the Mozilla JavaScript Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) for further reference.

Remember, you can [compile with babel][compile-modern-javascript-with-babel] to use all of this right now, it's very actively maintained and supports nearly everything.


[compile-modern-javascript-with-babel]: /javacript/tools/compile-modern-javascript-babel/
