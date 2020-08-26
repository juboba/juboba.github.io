
---
categories: javascript
date:  2016-03-17 11:36:00 +0100
title: Javascript, the misunderstood language
---
Without doubt this is the most misunderstood languages I know.
Javascript is a multi-paradigm language. So you can write declarative

So, let's begin with some simple stuff. This approach will be using ES6 (still a
draft at the time of this writing) as the standard.

# The types

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

```javascript
var myObject = {
  color: 'red',
  getColor: function() {
    return this.color;
  }
};
```


Objects can be created in several ways:

```javascript
var object = new Object;
var object = {};
var object = Object.create(Object); 
```

These are similar but not exactly the same (more on this later).

<!--
* When we use `Object.create()`, the object passed as
an argument to the function forms the prototype for the new object (more on prototypes in a second).
* When we use the `new` operator, the prototype of the operand (in this case `Object`) is used as the
prototype of the object created.
* When we use the Object literal [TODO]
-->


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

And as I said before, not returning anything actually returns `undefined`:

```javascript
console.log(myNewFunction()); // undefined
```

All functions are bound to objects, so if you don't specify an object, your
functions will be bound to the global object (`global` in Node, `window` in
the browser).

#### Scope

Every function (declared with the `function` keyword) defines a scope. So every
variable declared inside will only *live* inside the context of that function:

```javascript
function addOne(number) {
  var one = 1;
  console.log(one); // 1
  return number + one;
}

console.log(one) // undefined
```

Javascript uses something called [lexical scope](http://en.wikipedia.org/wiki/Lexical_scoping#Lexical_scoping)
to lookup variable declarations. This basically means that every variable is
available inside the function where it is declared but also inside the functions
declared inside the parent function.

```javascript
function parent() {
  var parent_var = 10;

  function child() {
    console.log(parent_var); // 10
  }
}
```

But! If the variable is redefined inside the function then it will be *shadowed*
by this new definition:

```javascript
function parent() {
var variable = 10;

  function child() {
    var variable = 5;
    console.log(variable) // 5
  }

console.log(variable) // 10
}
```

##### Globals

If you don't *declare* a variable and just start using it there are two posible
outcomes:

* The variable is being read. This will throw a `ReferenceError` saying the variable
is not defined.
* The variable is being written. This will define a *global property* with that name.

The last case can lead to confusion and contamination of the global scope.

```javascript
function myFunction(){
  some = 'my name';
  console.log(some); // 'my name' as expected
}

console.log(some) // 'my name'
console.log(window.some) // 'my name' (in browser)
console.log(global.some) // 'my name' (in Node)
```

You should **always** declare variables (even if you don't initialize them) using:

```javascript
var myVariable;
```

# Prototypal what?

Javascript is known by its prototypal inheritance. This is very different from other
languages.
If you come from PHP (or Java, or other Object Oriented Language) you may be used to
things like:

```php
<?php
class Person {
  public function __construct() {
    echo "Person created!";
  }
}

$john = new Person(); // Person created!
```

In this case the Person `class` specifies a *pattern* for objects to be created.
So every object in PHP is an **instance** of a **class**. But the Person class
itself is not an object, you treat the class very different from an instanced
object.

In Javascript there are no real classes (even though in ES6 we have the `class`
keyword). What we do have is `prototypes`.

So how does it work?

## Prototype chain

Every object has a prototype. You can think of the objects in Javascript as a
linked list in that every object has a *reference* to it's prototype. Every time
you ask for a property (this includes functions) from an object, the interpreter
will look for that property in the object, but if it doesn't find it there it will
look for the property in the object's prototype. If it's still not there, then the
prototype of the prototype will be searched for the property.
This process will occur until



# Expressions and Statements in Javascript

<!-- Here talk about declaration statements vs expression statements and the difference
between returning a value and not. -->

# Hoisting

Hoisting means that the interpreter will take the variable/function declaration and
'move' it to the top of the block.
Knowing this will help you avoid many `ReferenceError` problems.

Sometimes you will find code like the following:


```javascript
sayHello(); // hello!

function sayHello() {
  console.log('hello!');
}
```

The interpreter converts that code into this:

```javascript
function sayHello() {
  console.log('helllo!');
}

sayHello();
```

With variables something similar (yet not the same) happens:

```javascript
console.log(myVariable); // undefined

var myVariable = 'my text';
```

which is not the same as simply not defining the variable:

```javascript
console.log(myVariable); // ReferenceError
```

The reason is that the interpreter actually converts that code into this:

```javascript
var myVariable;

console.log(myVariable); // undefined

myVariable = 'my text';
```
