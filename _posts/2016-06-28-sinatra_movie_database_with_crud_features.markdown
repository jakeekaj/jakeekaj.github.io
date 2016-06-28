---
layout: post
title:  "Sinatra Movie Database with CRUD features"
date:   2016-06-28 16:14:26 +0000
---

This post was transferred from an older blog(Jekyll) dated 4/13/2016. 

So, I've finally decided to host my own blog on Github with the help of Jekyll (now with the help of Learn). They say every developer should have a blog. I'm just regretting the fact that I started late.

Part of the [Learn Verified](https://learn.co) Full Stack Web Development Program is creating a Sinatra CRUD app that has the following criteria:

- Build an MVC Sinatra Application.
- Use ActiveRecord with Sinatra.
- Use Multiple Models.
- Use at least one `has_many` relationship
- Must have user accounts. The user that created the content should be the only person who can modify that content
- Models must have validations to ensure that bad data isn't created
- Any validation failures must be shown to user with an error message


That sounds like a tough order, but it's actually not. We've been doing a lot of lab exercises prior to this assessment that deals with all these requirements. 

### The JMDB Sinatra App

![](http://imgur.com/j58LBUC.png)

I've decided to come up with a basic Sinatra CRUD app that deals with various models with several _has_many_ connections. As you can see, we have models for Movies, Genres, Shows, Actors, and a User. 

In a nutshell, here's the relationships that I had to make:

- A movie`has_many_ `actors, shows
- A show`has_many_` actors, genres
- An actor `has_many_ `movies, shows, genres
- A genre `has_many_` movies, shows, actors
- A user `has_many_` movies, shows, actors, genres

At first, it seemed to ba daunting task, but it's simply all about the Join tables and setting the schema right. When all that is set up, it's just a matter of fixing all your controllers to handle all the params properly. 

With regards to User authentication, I made sure to check if the current user is the one who made that entry, if not, he or she will not be able to EDIT or DELETE that entry. An error message will pop up:

![](http://imgur.com/dJHk4ur.png)


To make sure that all entries to the database are properly created, meaning bad data will not be entered, I've implemented validation checks to ensure that all models are properly created and saved. If, for instance, you tried to create a Movie without a Year, you will get this error:

![](http://imgur.com/Hp1IFc8.png)

Overall, I think I created an advanced version of the Sinatra Playlister Lab that was also part of the curriculum. The Playlister app only had three models, which was Songs, Genres, and Artists. Dealing with various models is challenging at first, but when you get the hang of it, it becomes natural. 

I'm happy with the Sinatra JMDB App. It has some limitations though and a lot of room for improvement. Here's a  [video](https://youtu.be/KV-Uea4N7U8) walkthrough of its features. 

Feel free to reach out if you have any questions or comments about this project.

