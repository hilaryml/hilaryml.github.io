---
layout: post
title:  "Adding jQuery to my Rails App"
date:   2017-02-03 14:51:35 -0500
---


For this assessment we were required to add some jQuery functionality to the app that we made for our previous assessment in Rails.

The app that I made for that assessment was a soccer league website that coaches and players could use to browse info related to their division, such as upcoming practices, games, teammate info and more.

I decided to add the required jQuery index resource to my teams index page, which displays the team names and their coaches. In order to do this I added a “Show Players” button which, when clicked, would display the player’s who are on that team.

I added the button in my team partial like so:

```
<button class="js-more" data-id="<%= team.id %>">Show Players</button>
```

And then placed the jQuery in the team index view:

```
<script type="text/javascript" charset="utf-8">
  $(function () {
    $(".js-more").on('click', function() {
      var id = $(this).data("id");
      $.get("/teams/" + id + "/players", function(data) {
        //format data
        var players = "<p><strong>Players:</strong></p>"
        $.each(data, function(index, player) {
          players += "<p>" + player.name + "</p>";
        })
        // Replace text of div
        $("#team-" + id + "-players").html(players);
      });
    });
  });
</script>
```

Woo! Onwards!

<iframe src="//giphy.com/embed/yTU8IQXHoBBUA" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/onward-yTU8IQXHoBBUA">via GIPHY</a></p>

Next, I moved on to add the jQuery show resource. In order to fulfill this requirement, I wanted to add a “Next” button to the game show view, so that players could look at upcoming games. This is where things got a little complicated.

At this point, while looking at the data that I was displaying for each game, I realized that I didn’t include form fields on the new Game form to actually add the teams that were playing, though I was clearly attempting to display that info on the game show view. I must’ve added in that info into the seeds, but forgot to allow the user to add it as well upon new game creation. Whoops! So I threw that in quickly in order to try to move forward with my next button.

I ran into an error here, whenever I clicked on the “Next” button, the info for the next game would start to show up and then be replaced again by the original text. I ended up adding in a preventDefault() function and this solved the issue.

<iframe src="//giphy.com/embed/3o7abB06u9bNzA8lu8" width="480" height="261" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/starwars-star-wars-the-force-awakens-bb-8-3o7abB06u9bNzA8lu8">via GIPHY</a></p>

In order to display the time and date for the games I needed to use the data to create a new Date js object and then format it. 

```
function getDateText(data) {
    var date = new Date(data["date"]);
    var day = date.getDate();
    var monthNum = date.getMonth();
    var months = [
      "January",
      "February",
      "March",
      "April",
      "May",
      "June",
      "July",
      "August",
      "September",
      "October",
      "November",
      "December"
    ];
    var month = months[monthNum];
    var time = new Date(data["time"]);
    var dateText = "Date and Time: " + month + " " + day + " at " + time.toLocaleTimeString();

    return dateText
  }
```

At this point my js script was getting quite large and I wanted to take it out of the game show view and place it in its own assets file. This brought up a whole other issue.

I not only took out the js script from the game show view, I also took out the script from the teams index page (where my “Show Players” button was). After all of this was nicely tucked away in separate files within my assets/js folder, and referenced in my application.js file, I started to notice that the page would no longer respond to my jQuery elements unless I reloaded it. After looking around stack overflow and trying out a bunch of different things (like wrapping it in $(document).ready, and so on), I reverted to what was working for me before and just placed the text back in the view files and tried to keep the hideousness to a minimum.

<iframe src="//giphy.com/embed/W9wHF6yVazlrW" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/hd-office-joke-W9wHF6yVazlrW">via GIPHY</a></p>

When I initially created this app, I allowed users to leave comments on games, thus fulfilling the last assessment’s many-to-many requirement. I wanted the comments associated with each game to show up while a user was clicking through games with the next button. My next button script ended up looking like this:

```
<script type="text/javascript" charset="utf-8">
  $(function () {
    $(".js-next").on("click", function(event) {
      event.preventDefault();

      var nextId = parseInt($(".js-next").attr("data-id")) + 1;
      var team = parseInt($(".js-next").attr("data-team"));
      $.get("/teams/" + team + "/games/" + nextId + ".json", function(data) {
        //replace text within html tags
        $("#gameId").text("Game " + data["id"]);

        var versusText = "Teams: " + data["teams"][0]["name"] + " vs. " + data["teams"][1]["name"];
        $("#versus").text(versusText);

        $("#location").text("Location: " + data["location"]);

        var dateText = getDateText(data);
        $("#dateTime").text(dateText);

        var comments = "<h4>Comments:</h4>";
        $.each(data["comments"], function(index, comment) {
          comments += "<p>" + comment.content + "</p>";
        })
        $("#comments").html(comments)

        // re-set the id to current on the link
        $(".js-next").attr("data-id", data["id"]);
      });
    });
  });
```

Lastly, I wanted to prevent a page reload when comments were created.

```
<script type="text/javascript" charset="utf-8">
  $(function() {
    $('form').submit(function(event) {
      //prevent form from submitting the default way
      event.preventDefault();

      var content = $(this).serialize();

      var posting = $.post('/comments', content);

      posting.done(function(data) {
        var comment = data;
        $("#comments").text(comment["content"]);
      });
    });
  });
</script>
```

I have to say, adding in the jQuery functionality definitely improved the feel of the site and overall user experience. 
