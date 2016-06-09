---
layout: post
title:  "Project: First Rails Assessment "
date:   2016-06-09 03:53:40 +0000
---

*A RESTful Rails app with complex features*

**Overview**

For my very first Rails project, I decided to go with Movie Database, which is in line with my Sinatra project, but with several added features. Primarily, users will be able to signup and create reviews of their favorite movies. An added bonus is that users will be able to rate the movies on the database, and the movie's rating will adjust accordingly. 

Here's a link to a [video](https://youtu.be/_qT66a3BnDU) overview.

Here's the Home page with Movies added:

![](http://i.imgur.com/Vt6LvrO.png?1)

**Features**

***Standard Devise authentication with Omniauthorization***

Thanks to Devise, all my authentication and omniauthorization are now established. Users can login via the normal route by creating an account, or they can sign-in using their Facebook account. 

***Join model between Movies and Users, which is the Review***

Using the `has_many, belongs_to, and has_many, :through` ActiveRecord associations between the three models. The Review, which is the Join model/table, has `:content` and `:rating` columns. 

***Nested Resources/routes and Nested Forms***

Reviews are nested resources for Movies, and nested forms are also established using a custom writer. 

***Validations for invalid data***

Standard validations for data are implemented such as presence of all fields, along with proper Year for the Movie model. You can't enter a year in the future or an invalid year in the past. 

Validation errors are also displayed accordingly, with highlights on the `fields_with_errors`.

***ActiveRecord scope methods to filter and sort the movie list***

You can sort movies according to rating, year released, or by being rated/unrated. 

***Dynamic rating system***

Rating is adjusted for each movie as a review is added or deleted. Average rating is calculated with 1 decimal place. I implemented a method that updates the rating of the movie each time a review is added, deleted, or edited. 






