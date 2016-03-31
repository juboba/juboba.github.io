---
layout: post
title:  "Javascript, first words"
date:   2016-03-15 11:36:00 +0100
categories: javascript
---

Without doubt this is the most misunderstood languages I know.
...

So, let's begin with some simple stuff. This approach will be using ES6 (still a
draft at the time of this writing) as the standard.

### The types

We have 6 primitive data types in javascript which are:

* Number
* String
* Boolean
* Null
* Undefined
* Symbol (new in ES6)

#### Number

The `Number` type is the only one in Javascript (there's no integers, floats, etc)
and it is has a double precision floating point format (binary64) (as specified by
[IEEE 754](https://en.wikipedia.org/wiki/IEEE_floating_point)). This means you can
'safely' use numbers between -2<sup>53</sup>-1 and 2<sup>53</sup>-1.
`Number` has three symbolic values:

* NaN
* -Infinity
* +Infinity

The only number with two representations is `-0` and `+0`. With `0` being the same
as `+0`. In practice, this has almost no consequences (you'll notice if you divide
by -0 or by +0 thoughm, *try it out!*)

#### String

The `String` type is used to represent textual data. As in other languages, a string
is an 'array' of integers and some array-like operations can be used on strings:


```javascript
var a = "my string";

console.log(a[1] === 'y'); // This is true
```

#### Boolean

`Boolean`s represent a logical type which can have either of two values: `true` and `false`.
In Javascript boolean values are always lower-cased.


#### Null

The only value that the `Null` type has is: `null`.

#### Undefined

Any variable in Javascript that has not been initialized with a value, has the value of `undefined`.
Also, any non-existing property of an object has the value `undefined` when tested:

```javascript
console.log(Object.myNonExistingProperty) // undefined
```

And any function with no return value returns `undefined`.

`undefined` is a property of the global object. You can check this out by doing (in the browser):

```javascript
console.log(window.hasOwnProperty('undefined'); // true
```

The initial value of `undefined` is the primitive value `undefined`.



### Objects

In Javascript, an `Object` is a collection of properties that can be referenced by an *identifier*.
Objects can be created in several ways:

```javascript
var object = new Object;
var object = {}
var object = Object.create(Object); 
```

These ways are similar but not exactly the same:

* When we use `Object.create()`, the object passed as
an argument to the function forms the prototype for the new object (more on prototypes in a second).
* When we use the `new` operator, the prototype of the operand (in this case `Object`) is used as the
prototype of the object created.
* When we use the Object literal [TODO]


### Functions

Functions are a first class citizen in Javascript. This means that functions can be assigned to variables,
passed as parameters for other functions or even returned by functions. This doesn't end here, as all of
these are perfectly legal in Javascript:

Assign a function to a variable:

```javascript
var a = function() {};
```

Pass a function as an argument to another function:

```javascript
function myFunction(fn) {
  fn();
}

var result = myFunction(function() {
  console.log("Hello!");
});

console.log(result) // undefined
```
Return a function from a function:

```javascript
function myFunction(fn) {
  return fn;
}

var result = myFunction(function() {
  return 'hello';
});

console.log(result) // function
console.log(result()) // 'hello'
```

You can even have arrays of functions:

```javascript
var myArray = [function() {return 'hello'}, function() {return 'goodbye'}];

myArray[0]() // 'hello'
myArray[1]() // 'goodbye'
```

So, functions are declared using the keyword `function`:

```javascript
function myNewFunction() {
}
``` 

And as we said before, not returning anything actually returns `undefined`:

```javascript
console.log(myNewFunction()); // undefined
```

All functions are bound to objects, so if you don't specify an object, your
functions will be bound to the global object (`global` in Node, `window` in
the browser).
