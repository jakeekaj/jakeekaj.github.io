---
layout: post
title:  "Intro to Custom Directives: Cleaning up the View"
date:   2016-07-09 06:39:43 -0400
---


For Angular newbies, Directives might sound intimidating at first. It can be a very complex topic if you really want to dive in. That's because Directives are very powerful when you know how to maximize its use. However, its basic goal is to create reusable elements and ultimately, to provide you with clean and readable code. 

Take a look at the sample code: 

***code may not be showing up properly due to the angular bindings `{{post.title}}` not being shown. Hit me on slack if it doesnt show. Thanks! @jakeekaj

```
<html ng-app='app'>
<body ng-controller='PostsController as ctrl'>
<ul>
  <li ng-repeat="post in ctrl.posts">
  <h4>Title: {{ post.title }}
  <h4>Content:</h4>
  <p>
    {{ post.content }}
  </p>
  <h5>Date posted: {{post.created_at}}</h5>
  <em ng-show="post.comments.length">{{post.comments.length}} comments(s)</em>
  </li>
</ul>
</body>
</html>
```
At the code above, we are basically using Angular's built-in directive called `ng-repeat` to iterate through the collection of posts. We have html elements set up to handle all the post's attributes such as the `title`,`content`,`date`, and even the number of `comments`. Of course, this is a very basic example of one part of your application. Is it easy to read? I guess you can say yes, but wait until you see how it looks when we make use of a custom angular directive.

```
<html ng-app='app'>
<body ng-controller='PostsController as ctrl'>
<ul>
  <li ng-repeat="post in ctrl.posts">
  <post-details></post-details>
  </li>
</ul>
</body>
</html>
```

From 7 lines of code from the original, we only have one line of code that shows up on our page. At first look, you can easily see what the Directive is for. It describes each post as we iterate through it with the help of `ng-repeat`. You probably noticed that we have an attribute of `post`. This is where we pass in the data from the `ng-repeat`. In other words, you can look at it as a gateway for the `post` data to our own custom directive. Let's now create that directive:


```
# postDetails.js

angular.module('app').directive('postDetails', function(){
  return {
    restrict: 'E',
    templateUrl: 'home/post.html'
  };
});
```

Take note of the name of the directive `postDetails`as compared to how we used it in our html view `<post-details>`. This is a convention that we have to follow. 

Let's break down on what's going on here. The first line simply `gets` our angular module `app` where we call our new directive which we set as `postDetails`. After that, we tell the directive to return an object with a `restrict` and `templateUrl`property. 

The `restrict` simply tells the directive that it should only be used as an Element and nothing else. The default for this is `A` which stands for attribute. 

The `templateUrl`will point to the file where we can find the content to be displayed by our custom directive. 

Our template file will look like this:

```
# home/post.html

<h4>Title: {{ post.title }}
<h4>Content:</h4>
<p>
  {{ post.content }}
</p>
<h5>Date posted: {{post.created_at}}</h5>
<em ng-show="post.comments.length">{{post.comments.length}} comments(s)</em>
```

In essence, we just cut out the content from out main html view into this partial html view. If you're coming from a Rails background, this is like making use of View Partials. 

It's always best practice to keep our views clean from clutter as much as we can, without affecting our site's functionality and useability. As I've said, there's a lot more about Directives including having its own `scopes` and `controllers`, but we will not cover that here. I just wanted to show you a very basic use of Angular Directives. It really helps in making your code much readable and easier to understand for other developers. 
