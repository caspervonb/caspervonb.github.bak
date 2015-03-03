---
layout: post
title: An Overview of JavaScript in 2015
categories: javascript
excerpt: ECMAScript 2015 (previously known as ECMAScript 6) is the upcoming version of the ECMAScript standard, the standard ...

redirect_from:
  - /2014/03/05/ecmascript-6-features-and-tools/
  - /2014/03/05/ecmascript-6-features-and-tools.html
  - /archive/ecmascript6-features-and-tools/
---

## Introduction

ECMAScript 2015 (previously known as ECMAScript 6) is the upcoming version of the ECMAScript standard, the standard previously said to be ratified in late 2013, then again in late 2014 is now targeting ratification in June 2015.

## Language Features

This edition of JavaScript introduces a lot of new language features, it might even be the most feature packed revision to date in terms of language additions, and that is with some of the features scheduled, like comprehensions and rest and spread properties being delayed to the next edition.

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
Classes are a concise declarative syntax for writing _classical_ object oriented prototype patterns, it has support for inheritance, super calls, instance and static properties and constructors.

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
```
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
Iterators allow iteration over arbitrary objects, any object can be an iterator as long as it defines a `next()` method, any object can be iterable as long as it defines an `iterator()` method.

Example:

```javascript
function RangeIterator(min, max) {
   this['iterator'] = function () {
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

```javascript
for (let i of new RangeIterator([1, 10])) {
   console.log(i);
}
```

### Generators
Generators make it easy to create iterators. Instead of tracking state yourself and implementing `iterator`, you just use yield (or yield* to yield each element in an iterator).

Example:

```javascript
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
```
obj = {
   toString() {
      return 'obj';
   },
};
```

### Modules
Modules provide declarative syntax for module patterns.

```js
export function isEqual(a, b) {
  return a == b;
}

export function isDeepEqual(a, b) {
   return a === b;
}

export default function assert(expression) {
  return expression == true;
}

assert.deepEquals = deepEquals;
assert.isEqual = isEqual;
```

```js
import assert, { isEqual } from './assert';
export * from './assert';
```

### Property Value Shorthand
Property Value Shorthand is a shorthand syntax object initializers whose property keys are initialized by variables of the same name, whereas before we would have to repeat the property key and variable name.

```js
function f( x, y ) {
  return { x: x, y: y };
}
```

We can now drop the redundant repetition completely
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
```javascript
var x = 1;
var y = 2;
console.log(`${ x } + ${ y } = ${ x + y}`); // => "1 + 2 = 3"
```
