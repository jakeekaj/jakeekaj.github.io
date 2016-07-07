---
layout: post
title:  "Project: Angular and Rails"
date:   2016-07-07 00:22:40 -0400
---


Getting started was probably the hardest part of this project. Connecting Angular and Rails is a challenge because there are many approaches you can take. After reading dozens of online resources, I ended up following Luke Ghenco's [Guide](https://medium.com/@lukeghenco/create-an-angular-js-app-with-a-restful-rails-api-pt-2-8fbc7177e334#.l1a8merhq) on Medium which was very clear and straightforward. 

**The Back-end: Rails API**

Since I previously worked on `Movie` and `Reviews` model for my last two projects, I decided to use the same model as well. `Review` `belongs_to` a `Movie`and a `Movie has_many Reviews`. Seems pretty simple, right? I decided to make use of a nested resource to have RESTful routes in the process. 

**JSON** 

Since we will be relying on JSON data, we simply need the Rails API to handle all these. Inside the Movies and Reviews Controllers back in Rails, I simply had to include the code `respond_to :json` in order to make sure that we generate JSON when working with our models. 

**Routes**

To make sure that proper routes are configured, here's my routes.rb file:

```
Rails.application.routes.draw do
  root 'application#index'
  namespace :api, defaults:{format: :json} do
    namespace :v1 do
      resources :movies do
        resources :reviews
      end
    end
  end
end
```

As you can see, I included a `namespace` to indicate that we are working an API. `:movies` and `:reviews` are nested accordingly. We still have to set root route so that our angular front-end has an entry-point. After setting the route, here's the file on 

'app/views/application/index.html.erb'

```
<div ui-view></div>
```

The `ui-view` is a directive that's part of the ui-router angular module. It takes care of the routing on the front-end of the app. Now that we've buil the back-end, we can now work on the front-end. 


**The front-end: Angular JS**

By using the `gem 'bower-rails'`in our Gemfile, I have now access to the Angular modules that I need. More info on this from Luke's [Guide](https://medium.com/@lukeghenco/create-an-angular-js-app-with-a-restful-rails-api-pt-2-8fbc7177e334#.l1a8merhq). 

**front-end routing by UI-Router**

To take advantge of the UI-router's features, here's my app.js file:

```
var app = angular.module('myApp', ['ui.router', 'templates']);


app.config(function($stateProvider, $urlRouterProvider) {
   $stateProvider
     .state('home', {
       url: '/',
       templateUrl: 'home.html',
       controller: 'HomeController as ctrl'
     })
     .state('home.new', {
       url: 'new',
       templateUrl: 'home/new.html',
       controller: 'NewMovieController as ctrl'
     })
     .state('home.movies', {
       url: 'movies',
       templateUrl: 'home/movies.html',
       controller: 'MoviesController as ctrl'
     })
     .state('home.movie', {
       url: 'movies/:id',
       templateUrl: 'home/movie.html',
       controller: 'MovieController as ctrl'
     })
     .state('home.review', {
       url: 'movies/:id/review/new',
       templateUrl: 'home/review.html',
       controller: 'ReviewController as ctrl'
     });
  $urlRouterProvider.otherwise('/');
});
```

As you can see, it's a simple setup with 5 views. Nested views is implemented in the process of adding a review and showing a movie. 

In order for us to access the hit the API and send `get` and `post` requests, I made use of a simple Angular Service that makes $http requests:

```
app.service('RestfulService', RestfulService);

function RestfulService($http){

  this.getMovies = function(){
    return $http.get('http://localhost:3000/api/v1/movies');
  };

  this.addMovie = function(movie){
    return $http.post('http://localhost:3000/api/v1/movies',movie);
  };

  this.deleteMovie = function(movieId){
    return $http.delete('http://localhost:3000/api/v1/movies/'+ movieId);
  };

  this.getMovie = function(movieId){
    return $http.get('http://localhost:3000/api/v1/movies/'+ movieId);
  };

  this.addReview = function(movieId, review){
    return $http.post('http://localhost:3000/api/v1/movies/'+ movieId + '/reviews',review);
  };


};

```

I must say that this can be considered the "heart" of the app as it takes care of all the requests to the Rails API backend. 


**Sort with Angular Filters**

Filtering is made quite easy with the help of Angular Filters. Filtering is quite easy to setup since Angular is designed to be that way. 

These two buttons will take care of the sorting with the help of `sort_data_by()`method. 

'app/assets/javascripts/templates/home/movies.html'
```
...
 <button class="btn btn-primary"  ng-click="ctrl.sort_data_by('title')">Title</button> |
 <button class="btn btn-primary"  ng-click="ctrl.sort_data_by('reviews')">Reviews</button>
  ...
```

The data will then be passed on in the controller as such:

'MoviesController.html'

```
  ctrl.sort_data_by = function(name){
      ctrl.sort_on = name;
      ctrl.reverse = !(ctrl.reverse);
    }
```

By looking at the code,  `sort_on` takes the name of the argument that's passed in depending on the button that's clicked. This will then be applied to our `ng-repeat` directive which takes care of iterating through our data.

'movies.html'

```
...
<ul class="list-group">
  <li class="list-group-item" ng-repeat="movie in ctrl.movies | filter: ctrl.search | orderBy: ctrl.sort_on : ctrl.reverse ">
    <h4>Title:<a ui-sref='home.movie({id: movie.id})'> {{ movie.title }}</a>
      <span class="glyphicon glyphicon-remove" style="cursor:pointer" ng-click='ctrl.delete(movie.id)'></span></h4>

    <h4>Plot:</h4>
    <p>
      {{ movie.plot }}
    </p>
    <em ng-show="movie.reviews.length">{{movie.reviews.length}} review(s)</em>

  </li>
</ul>
...
```

As you can see, Angular filters are pretty handy to work with. You can create custom filters to ensure that your data can be sorted accordingly. The `orderBy:` filter orders an array depending on the expression passed in. 

**Final Words**

Overall, this is a basic Angular App that takes advantage of the Rails API built in the back-end. Since this is just a demo app to show basic capabilities of Angular, I decided not to include authorization at this time. Angular can be a very powerful tool in building fast and reliable web apps. Functionality can easily be added with the help of Angular Modules. 






