---
layout: post
title:  "Project: Expanding my Rails App with jQuery features"
date:   2016-06-17 05:27:57 +0000
---


My very first Rails App was a tough one. While the models are pretty basic, I must admit that it was still a challenge to make. However, when I found out that the next Project was to build on my app with new features, it sounded easy. When I looked at my code, it was the total opposite and I seriously considered starting from scratch. 

***On using jQuery and JSON***

I figured that the best way to make use of JSON and jQuery is at the index page for movies. This is the home page where users are directed to after logging in.  

[Imgur](http://i.imgur.com/IgpxRIA.png)

Since we are dealing with a nested resource of movies and reviews, I decided to load the index of all reviews for each movie. I simply had to add a link at each movie entry, if they have reviews. The index action for Reviews is defaulted to a json page, which we can use to render the details of each review on the index page. Without any page refreshes and redirects, I am able to load all the reviews for each movie.

[Imgur](http://i.imgur.com/zEtOc8H.png)

With the help of the `<script>` element at the bottom of the index page, I am able to "hijack" the `Show Review` link and load the reviews from the background.

```
<script type="text/javascript" charset="utf-8">
$(document).ready(function(){


// activate listener to show reviews and put on the next row
  $('a.showReviews').on("click", function(e){

    $.get(this.href).success(function(response){
      //grab the JSON response and parse to get the required data
    $.each(response, function(index,review){
        var html = "<tr style='color:red;'>";
        html += "<td><strong>Review:</strong></td><td colspan='2'><em> " + review['title'] + "</em> &nbsp; &nbsp; ("+review['rating']+"/10)</td>"
        html += "<td><strong>Reviewer:</strong></td><td colspan='2'><em>" + review['user']['email'] + "</em></td></tr>"

        $("a#showReviews-"+review['movie']['id']).hide();
        $("#movieDetails-"+review['movie']['id']).after(html);

      });
    });
    e.preventDefault();
  });
});
</script>
```
Since the response is already in JSON format, it is quite easy to convert it to a Javascript Object and add it to the DOM so that we can view the resource without reloading the page. 

I also applied the same principle in rendering the Contents of each Review, this time on the Movie Show page. 

[Imgur](http://i.imgur.com/4Fq3m8Q.png)

To achieve this, I had to make sure to render the Show action for the Reviews Controller, the right way. 

```
def show
    respond_to do |format|
      format.html { render :show }
      format.json { render json: @review, serializer: ReviewSerializer }
    end
  end
```

Thanks to the Active Model Serialization gem, I am able to select which info to return. This made it easier for me to access the data that I need, which is the content for the Review. 


***On creating a new resource without any page reloads***

I had a hard time in adding new features on the current Movie index and show pages. There was simply a lot going on in each page because most of the resource can already be accessed with just the two actions. That's why I decided to create a new resource just for this feature. I added a new Model which is a `Quote` resource, that simply `belongs_to` a Movie. A movie `has_many` Quotes, obviously. 

[Imgur](http://i.imgur.com/V4GSYJh.png)

As you can see, you can easily add new Quotes to the Movie with the form at the bottom. This form is "hijacked" since when you submit it, it won't direct you anywhere. It just creates the resource, and instantly adds it the DOM with the help of the JSON response that we made use of.

```
$("#new_quote").on("submit", function(e){

    url = this.action

    data = {
      'authenticity_token': $("input[name='authenticity_token']").val(),
      'quote':{
        'text': $("#quote_text").val(),
        'movie_id': $("#quote_movie_id").val()
      }
    };

    $.ajax({
      type: "POST",
      url: url,
      data: data,
      success: function(response){
        var html = "<li>&quot;"
        html += response['text']
        html += "&quot;</li>"
        $('div#quotesList ul').append(html)
        $('#quote_text').val("")
      }
    })

    e.preventDefault();
  })
```

This is a bit of a crude way of doing it, but it simply gets the job done while avoiding any errors. With the help of AJAX, I am able to create a new resource and then make use of the response by adding it in the DOM. 

You can watch all of these features in action, in my [walkthrough video](https://www.youtube.com/watch?v=HGE-u5xnTbQ). 


