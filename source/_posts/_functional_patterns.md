---
title: Functional Patterns for frontend development
---
Frontend development is pretty 'safe'. Most of the time the worst thing that can
happen is a poor user experience, but in the end, the real bad things can happen
in the backend (data inconsistency, security breach, permission issues, etc).

But nowadays people are really picky with web applications, right? I mean, if
you get the same error twice, you might as well close the application and
quickly look for an alternative (and most probably you'll find one or two!).

It's not really necessary to explain a lot to conclude that we should always aim
to give the best user experience when developing an application.

In this quest to prevent Runtime errors, I kept finding different principles and
strategies coming from the functional paradigm. Runtime errors are not domain
bugs (user count number is wrong or incorrect ID was provided...) but errors
that will make your application crash (cannot get property 'name' of undefined).

## Programming for humans, not for machines

## The case for *null**
[Tony Hoare introduced Null references in ALGOL W back in 1965 "simply because it 
was so easy to implement", says Mr. Hoare. He talks about that decision
considering it "my billion-dollar mistake".](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare/)

So this guy, invented Quicksort, smart guy, says that `null` is he's billion
dollar mistake: Well, Javascript has two of them <sup><a id="fnr.3" class="footref" href="#fn.3">3</a></sup>, they're not the same, can't be used interchangeably and
certainly don't give much information.

We don't want them, we don't really need them. Instead we'll use a simple
strategy that can certify that we always have a value and that we always need to
treat the "nothingness" of an object.

We've all experienced what happens when you receive a object from 'somewhere' and
you need to access a certain nested property from it.

{% codeblock lang:javascript%}
const getUserFullName = user => `${user.name.first} ${user.name.last}`
{% endcodeblock %}

It's a one-liner, not an ugly function, but it explodes when `user === null`, or
if the API decides that now the trend is `user.firstName` and friends.

{% codeblock lang:javascript%}
> user.name.first
Thrown:
ReferenceError: user is not defined

> const user = { firstName: 'Bob', lastName: 'Bobber' }
> user.name.first
Thrown:
TypeError: Cannot read property 'first' of undefined
{% endcodeblock %}

A common solution to the problem is:

    user && user.name && user.name.first

No, this can't be the way.

So instead, let's think about one pattern that never fails in javascript:


<a id="org40ec422"></a>

### Array.Prototype.map

`map` is an interesting method because it never fails<sup><a id="fnr.4" class="footref" href="#fn.4">4</a></sup>. Probably you use it a lot with libraries like
`React` for rendering repeated elements:

{% codeblock lang:javascript%}
return (
  <div className="user-list">
    { users.map(user => <UserAvatar key={user.id} user={user} />) }
  </div>
)
{% endcodeblock %}

And we don't really care if the list is empty. We just don't get any elements
rendered. In most cases, the only thing left is to add a special case to show an
"empty list" message to the user <sup><a id="fnr.5" class="footref" href="#fn.5">5</a></sup>.

But let's look at the power of behind this safety:

{% codeblock lang:javascript%}
// If there are no Pauls, we get an empty array: []
const getPauls = users => users.filter(u => u.name === "Paul")
const getAge = user => user.age

const getPaulsAges = users => getPauls(users).map(getUserFullName)
{% endcodeblock %}

The `getPaulsAges` is guaranteed to go ok as long as `users` is an array.
We don't even care what's in that array anymore.
If one user doesn't have an age we'll see `undefined` in the user's position,
but this has an easy solution:

{% codeblock lang:javascript%}
const pauls = [{ name: 'Paul', age: 4 }, { name: 'Paul' }]
const validaAges = getPaulsAges(pauls)
  .filter(age => !!age) // discard all 'falsy' ages.
{% endcodeblock %}
