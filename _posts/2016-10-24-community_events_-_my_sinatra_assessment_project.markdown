---
layout: post
title:  "Community Events - My Sinatra Assessment Project"
date:   2016-10-24 23:13:29 +0000
---


As I sat mulling over ideas for my Sinatra assessment, a thought occurred to me. What if I made an app that allowed users to post items that they wanted to trade with other users? What if users could then find items that they wanted and swap for them? WHAT IF I called this fantastic app SWAPP. As this glorious idea rolled over me, I started to think, “…what are the odds that someone else has already thought of this?”

Turns out, those odds are pretty good. (See [here](https://itunes.apple.com/us/app/swapp-it/id982178939?mt=8), and [here](https://www.swap.com/), oh and [here](http://www.swapsity.ca/) too.)


Back to square one.

Okay, so maybe I’m not going to be able to come up with any brilliant ideas for a crazy new app that no one has ever before conceived all in one afternoon. Maybe, just maybe, I could come up with a simple app that is efficient and effective at doing its own simple little thing.

As I mulled all this over, I noticed my newspaper in the corner and started thinking about plans for the weekend. I’m still relatively new to this community and it would be nice to do a little exploring, experience the people and the ‘feel’ of this new city. Actually, it would be super cool if there were a resource that listed all of the public events that were going on. Everything from fundraisers, to concerts, to agricultural fairs…

Enter Community Events.

Now as someone who gets pretty restless on a regular basis, I often find myself google searching for interesting things that are happening locally, so the idea for this app really caters to me.

I decided to get started by scratching out all the ideas bouncing around my head onto a piece of paper. Always a smart idea if you want to avoid insanity.

Next I setup the basic file structure I would need for my app, and coded my migrations and models.

I knew that I wanted to validate the presence of both the events’ and users’ attributes, as well as encrypt passwords, so I threw all that in there.

```
#event.rb
class Event < ActiveRecord::Base
  belongs_to :user
  validates_presence_of :title, :date, :location, :description, :contact
end

#user.rb
class User < ActiveRecord::Base
  has_secure_password
  has_many :events
  validates_presence_of :username, :email, :password
end
```


I knew right off the bat that I wanted a layout page that would keep the title of my app as well as it’s tagline at the top of the page.

```
#layout.erb
<!DOCTYPE html>
<html>
  <head>
    <title>Community Events</title>
  </head>
  <body>
    <h1>Welcome to Community Events!</h1>
    <h4>Hamilton's source for local entertainment!</h4>
    <%=yield%>
  </body>
</html>
```


Next, I started building out my session controller and helper methods, and then my signup and login view pages and their controller routes. 

```
#session_controller.rb
helpers do

    def logged_in?
      !!current_user
    end

    def current_user
      current_user ||= User.find_by(email: session[:email]) if session[:email]
    end

    def login(username, password)
      @user = User.find_by(username: username)
      if @user && @user.authenticate(password)
        session[:email] = @user.email
        redirect "/"
      else
        redirect "/login"
      end
    end

    def logout
      session.clear
    end
		
end
```


After all that, I got started on my event controller and view pages. At this point I started really thinking about functionality and how I wanted a user to navigate through the app (things like where I wanted them to end up after creating or editing a new event, etc). After trying a couple different layouts, I settled on my current pattern. I threw links at the top of most pages so users could navigate through the site in endless circles, without having to resort to the use of the browser’s back button. I didn’t want the same links showing up if a user was logged in versus just scoping out the events page. So I created an instance variable @links and just set it equal to whichever links fit the situation, through the use of if/else statements.

```
#session_controller.rb
get '/' do
    if logged_in?
      @links = {"Logout": "/logout", "Your Events": "/events", "New Event": "/events/new"}
    else
      @links = {"Login": "/login", "Signup": "/signup"}
    end
    @events = Event.all
    erb :index
  end
```


Then I just iterated through the links in the index view and displayed them at the top of the page.

```
#index.erb
<% @links.each {|key, value| %>
  <a href="<%=value%>"><%=key%></a>
<% } %>
```

During all of this I was constantly running shotgun in between my commits to github in order to test out various aspects of the app. This helped me tweak a lot of the formatting and fix a couple random bugs. 

One of the things that occurred to me during a stint on shotgun, was that all of the events were displaying in the order that they were created (as expected), and not in the order that they would occur throughout the year (as desired). So I sorted those suckers by date. 

```
#index.erb
<% @events.sort{|a,b| a.date <=> b.date}.each {|event| %>
  <a href="/events/<%=event.id%>"><%=event.title%></a>
  <p><%=event.date%></p>
…
```

Ooooh preeetty.


I also decided to throw in some handy-dandy flash messages, to yell at users when they’re doing things wrong.


Overall, I'm pretty happy with how my app turned out. I feel that it's something that would appeal to me both as a user and a member of the community.
