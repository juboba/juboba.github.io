---
layout: post
title:  "AngularJS 1.5.x Primer"
date:   2016-03-15 11:36:00 +0100
categories: javascript angular
---

AngularJS is a Single Page Application Framework (SPA from now on).
It differs a lot from other libraries like jQuery, MooTools, etc in that
it is really a framework and not a library.

Being a framework basically means that we will be working inside Angular's
specification instead of using angular tools to give our app some kind of
(isolated?) functionality, like we would do with jQuery:

{% highlight javascript %}

$('#button').click(function(event){
    console.log(this);
});

{% endhighlight %}

This piece of funcionality is totally isolated from an application context.
jQuery is not asking we to do things "the jQuery way". In fact, that's a cool
thing about jQuery: we can structure our application as we want without having
to comply to any pre-defined structure.

AngularJS is a totally different thing. If you've worked with PHP frameworks like
Laravel, CodeIgniter, etc or with Python frameworks such as Django or Flask you
noticed the frameworks run our code in a context. It's not we saying exactly when
to run what, it is the framework figuring out what code should it run when some event
(like a request) has been fired.

### Baby steps

So let's see some code:

{% highlight html %}
<html>
    <head>
        <link href="style.css" rel="stylesheet"/>
    </head>
    <body ng-app>
        <span ng-bind="myVariable"></span>
        <input type="text" ng-model="myVariable"/>

        <script src="angular.min.js"></script>
    </body>
</html>
{% endhighlight %}

Here we have a typical `html` document with some new stuff:

* **ng-app** Tells angular that it should be aware of the things that happen inside
this element.
* **ng-bind** Sets one-way binding to this element; meaning that any changes to the
`myVariable` variable will be automatically reflected in the view.
* **ng-model** Sets two-way binding to this element. This means that any changes to
the value of this element will notify angular to refresh every place where `myVariable`
is used.


So, what do we have here?

Angular comunicates with templates through **directives**. Directives are a way of
teaching HTML new features.
Angular looks for a `ng-app` directive to bootstrap our application. So if we omit
this directive, Angular won't take any action in our document.

We can go ahead and try it.

### Get AngularJS

We can get Angular from different sources. I prefer to use the Node Package Manager
to install it:

```
$ npm install angular
```

This will install angular in the `node_modules` subdirectory in our app. We can go
inside and look at the files.
Now we should include it in our document:

{% highlight html %}
<html>
    <head></head>
    <body>
        <script src="node_modules/angular/angular.min.js"></script>
    </body>
</html>
{% endhighlight %}


## Main concepts

When building SPA's we want to move logic from the server to the client. There are
many reasons for this change in web development:

* Client devices are much more powerful than ever before. We have better computing
capacity in our pocket with our mobile phone or with an mp3 player than what we
had 15 years ago with a full office computer.
* We free the server from doing front-end related computing (ordering data to be
shown by date, computing div sizes, etc).
* Packets of transfered data are smaller as we don't have to request a full page
with images and structure. We just ask for the *contents* to fill in the blanks.
* User experience enhancement: People don't have to wait so long to load the
application. Gmail is an example of this. Some of the actions of our application
can even be available in offline mode!

To get us on the way of building SPA's we'll need more things on the client than
before, because we need to do things (i.e. routing) that tipically were done on the
server side.

As a basic set, we'll need:

* A way of creating web-components.
* A url router to navigate through our application.
* A way of connecting our view-models with our view (data binding).

Data binding is an interesting to have. Tipically we have to take care of keeping
in sync the data from our models (javascript objects) with the data that is being
displayed to the user (existing objects in the DOM). But if the framework could take
care of this for us it would be very nice.

AngularJS gives us a set of tools to meet these requirements and more. The first things
we should get familiarized with in an AngularJS environment are:

* **Controllers**: These are the basis for creating web components. Controllers aim
to control (duh!) pieces of the application tipically linked with a view. For instance,
a controller may be in charge of displaying what is needed in a sidebar.
* **Directives**: In my opinion directives are a first class citizen when speaking
about SPA's. Directives can be much more isolated than controllers and allow the developer
to define reusable pieces of functionality like a user widget or the structure for a post.
The new frameworks like Angular2 have dropped controllers in favour of directives or web
components.
* **Services**: Need to fetch data from the server? Need to share data between
controllers/directives/etc? Need to group global functionality? This is the place to do it!
* **Filters**: Sometimes you want to show data in a different way of how it's stored.
Filters act as one-way translators from the model to the view (we have other ways of
doing the opposite).
