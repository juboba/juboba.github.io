---
layout: post
title:  "AngularJS 1.5.x Primer"
date:   2016-03-15 11:36:00 +0100
categories: personal
---

AngularJS is a Single Page Application Framework (SPA from now on).
It differs a lot from other libraries like jQuery, MooTools, etc in that
it is really a framework and not a library.

Being a framework basically means that you will be working inside Angular's
specification instead of using angular tools to give your app some kind of
(isolated?) functionallity, like you would do with jQuery:

{% highlight javascript %}

$('#button').click(function(event){
    console.log(this);
});

{% endhighlight %}

This piece of funcionality is totally isolated from an application context.
jQuery is not asking you to do things "the jQuery way". In fact, that's a cool
thing about jQuery: you can structure your application as you want without having
to comply to any pre-defined structure.

AngularJS is a totally different thing. If you've worked with PHP frameworks like
Laravel, CodeIgniter, etc or with Python frameworks such as Django or Flask you
noticed the frameworks run your code in a context. It's not you saying exactly when
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

You can go ahead and try it.

### Get AngularJS

You can get Angular from different sources. I prefer to use the Node Package Manager
to install it:

```
$ npm install angular
```

This will install angular in the `node_modules` subdirectory in your app. You can go
inside and look at the files.
Now you should include it in your document:

{% highlight html %}
<html>
    <head></head>
    <body>
        <script src="node_modules/angular/angular.min.js"></script>
    </body>
</html>
{% endhighlight %}

