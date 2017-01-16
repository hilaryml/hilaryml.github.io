---
layout: post
title:  "Incorporating JS into my Custom Browser Start Page"
date:   2017-01-16 22:32:15 +0000
---


A while back I posted a blog about a personal project that I worked on - a custom browser start page.

I've just finished the JS unit and decided to incorporate a couple things into the project.

For starters, I wanted to play around with the clock function as well as format the date. 

In order to accomplish the first task, I took out the original clock hack that I'd found somewhere in the depths of stack overflow, and started my own from scratch. It ended up being pretty simple. Similar to Ruby, JS has a Date object that you can play around with. 

```
function displayTime() {
  var clockDiv = document.getElementById('clock');
  var time = new Date().toLocaleTimeString()
}
```

The above code would give me the current time, and would format it too! Sweeeet.

Next I had to add the date to the clock div on my page so that it would actually show up.

```
//displayTime()
...

  clockDiv.innerText = time
}
```


Lastly, I had to make my clock 'tick'. In order to do this, I needed to call on the clock function every second and have the new time display to the page.

```
$(document).ready( function() {
  setInterval(displayTime, 1000);
}
```

Next, I needed to format my date. This proved to be a bit more involved as I couldn't find the Ruby equivalent of .strftime.

I ended up doing the following:

```
function displayDate() {
  var currentDate = new Date();

  var day = currentDate.getDay(); //returns number between 0-6
  var days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

  var date = currentDate.getDate();

  var month = currentDate.getMonth(); //returns number between 0-11
  var months = ["January", "February", "March", "April", "May", "June", "July",
    "August", "September", "October", "November", "December"]

...
```

So in JavaScript, you use functions like getDay(), getMonth() and so on to grab the aspects of your Date object that you want to play around with. However, rather than returning strings, these functions return integers representing what they are. For instance, if I used getDay() to return the day of the week, it would return "1" today, because today is Monday. In order to convert these integers into their counterparts, I placed the days of the week in an array and used the integer return value as an index to grab it's counterpart out of the array.

I repeated this process for the months.

Next, I wanted to add 'st', 'nd', and other endings to the day of the month in order to convert them into ordinal numbers rather than cardinal numbers (ex. 1 to 1st, 2 to 2nd, etc). I did so by taking the modulus of the day in order to check to see what its last digit was.

```
...

//append 'st', 'nd', 'rd', or 'th' depending on date
  if (date % 10 === 1) {
    var ending = "st";
  } else if (date % 10 === 2) {
    var ending = "nd";
  } else if (date % 10 === 3) {
    var ending = "rd";
  } else {
    var ending = "th";
  }
	
  var html = days[day] + " " + months[month] + " " + date + ending
  return html

}
```

Next, I decided to add some social media links.

I found some free icons online and added them to my images folder. Next I made them into links in my index.html page like so:

```
<div class="social-media">
  <a href="https://www.facebook.com/" id="facebook"><img src="images/facebook.png"></a>
  <a href="https://www.instagram.com/" id="instagram"><img src="images/instagram.png"></a>
  <a href="https://www.pinterest.com/" id="pinterest"><img src="images/pinterest.png"></a>
  <a href="https://youtube.com/" id="github"><img src="images/youtube.png"></a>
  <a href="https://www.linkedin.com/" id="linkedin"><img src="images/linkedin.png"></a>
  <a href="https://github.com/" id="github"><img src="images/github.png"></a>
</div>
```

I think in the future, I'd like to play around with making an image carouself to display a different background image every 20 seconds...

[Here's](https://github.com/hilaryml/browser-start-page) the link to my full repo. Feel free to clone it and try it out for yourself!

![Final product](http://i.imgur.com/rQqaEF5.png)
