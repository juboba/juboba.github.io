
---
categories: javascript
title: Introduction to lenses in Javascript
---

# Working with lenses

Lenses are a way of accessing values in nested structures. Creating a lens returns an object (a function) you can use to `view`, `set` or map a function `over` it.

We'll use **ramda**'s functions to show some uses of lenses.

**NOTE**: Since we haven't talked about [currying](https://javascript.info/currying-partials#currying-what-for), I will specify every parameter in every function call. This can

## Creating a lens

There are several ways of creating a lens, but the simplest are:

```js
import { lensProp, lensPath } from 'ramda'
```

-   Creating a lens from a prop ([lensProp documentation](https://ramdajs.com/docs/#lensProp)):

```js
const nameLens = lensProp('Name') // will point to: object.Name
```

-   Creating a lens from a path ([lensPath documentation](https://ramdajs.com/docs/#lensPath)):

```js
const streetLens = lensPath(['Address', 'Street']) // will point to: object.Address.Street
```

We don't need to care about what these objects are (they are just some 'weird' functions, but we won't use them directly).

Let's see how to use them with the following object(s):

```js
const collaborators = [
  {
    Name: 'George Gamow',
    Photo: null,
    Address: {
      Street: null,
      Number: 2
    }
  },
  {
    Name: null,
    Photo: 'http://nopicture.com/images/2',
    Address: {
      Street: null,
      Number: 2
    }
  },
  {
    Name: 'Emmy Noether',
    Photo: 'arst',
    Address: {
      Street: 'my street',
      Number: 2
    }
  }
]
```

Let's say we want to see what a certain value is:

```js
import { view } from 'ramda'
  console.log(view(nameLens, collaborators[0])) // George Gamow
  console.log(view(streetLens, collaborators[0])) // null :(
```

Now, let's replace that null with a value:

```js
import { set } from 'ramda'

const output = set(streetLens, 'No street provided', collaborators[0])

console.log(output)
```

```js
{
  Name: 'George Gamow',
  Photo: null,
  Address: {
    Street: 'No street provided',
    Number: 2
  }
}
```

With the help of `map` we can do it for all elements:

```js
const output = collaborators.map(collaborator => set(StreetLens, 'No street provided', collaborator))

console.log(output)
```

```js
[
  {
    Name: 'George Gamow',
    Photo: null,
    Address: {
      Street: 'No street provided',
      Number: 2
    }
  },
  {
    Name: null,
    Photo: 'http://nopicture.com/images/2',
    Address: {
      Street: 'No street provided',
      Number: 2
    }
  },
  {
    Name: 'Emmy Noether',
    Photo: 'arst',
    Address: {
      Street: 'No street provided',
      Number: 2
    }
  }
]
```

Whoops! we lost the Address.Street value for the last element. Let's use `over`, to provide a function instead of a value:

```js
import { defaultTo, over } from 'ramda'

const getStreet = value => defaultTo('No street provided', value)
const output = collaborators.map(collaborator => over(StreetLens, getStreet, collaborator))

console.log(output)
```

```js
[
  {
    Name: 'George Gamow',
    Photo: null,
    Address: {
      Street: 'No street provided',
      Number: 2
    }
  },
  {
    Name: null,
    Photo: 'http://nopicture.com/images/2',
    Address: {
      Street: 'No street provided',
      Number: 2
    }
  },
  {
    Name: 'Emmy Noether',
    Photo: 'arst',
    Address: {
      Street: 'my street',
      Number: 2
    }
  }
]
```

You can read more about the `defaultTo` function [here](https://ramdajs.com/docs/#defaultTo)

Since ramda's `path` can also use numbers in the provided array, we can create a lens to act on the n-th element of a list.

Let's imagine the shape of the user was this instead of the previous one:

```js
{
  Name: 'Emmy Noether',
  Photo: 'arst',
  Addresses: [
    {
      Street: 'my street',
      Number: 2
    },
    {
      Street: 'another street',
      Number: 9
    },
    {
      Number: 8
    },
    {
      Street: null,
      Number: 3
    },
  ]
}
```

You could act on the street of the first address with:

```js
const firstAddressStreetLens = lensPath(['Addresses', 0, 'Street'])
```

## Lenses in pipelines:

Lenses can be used in pipelines with tiny functions that will, for instance:

-   `setDefaultBackground`: set a default image if the background is not set.
-   `convertToHex`: convert a color to hexadecimal from some other format.
-   `toUppercase`: convert a string to uppercase.

So, we're taking functions that receive strings and return strings to process a whole object:

```js
const myPipeline = genially => pipe(
  genially => over(backgroundLens, setDefaultBackground, genially),
  genially => over(colorsLens, convertToHex, genially),
  genially => over(ownerCollaboratorsNameLens, toUppercase, genially),
  genially => over(secondCollaboratorsNameLens, toUppercase, genially)
) (genially)
```

In this case, the order of the tasks is irrelevant (unless two lenses act on the same part of the tree).

## Composing Lenses

Lenses can be composed in many ways. So if you have a lens that gets you to the path: `genially.collaborators[0].account` and another one that gets you from an account to a certain value inside, you can compose them to produce a lens that goes all the way inside.

## Notes

Something to note is that this type of access is safe in the following ways:

-   If the path property does not exist, the function provided to `over` won't run (just as when you run `map` on an empty list).
-   If the path to the property does not exist, `set` will create the path (but will create objects all the way, not arrays. You can compose lenses to get other results).
-   If the path to the property does not exist `undefined` is returned by `view`.
