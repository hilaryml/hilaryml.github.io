---
layout: post
title:  "Creating a Custom Browser Start Page"
date:   2016-10-27 00:21:52 -0400
---

After viewing several of Avi's video lectures, I became enamored with his browser's custom start page and wanted to figure out how to create one for myself. 

After several hours of research, and a considerable amount of trial and error, I figured I would share my findings in case anyone else shares my desire for a super cool custom start page.

Now, before I get started, I noticed that there were a bunch of different ways to go about this that went way over my head. I ended up just winging it and trying to keep things as simple as possible, and this seemed to work out okay for me. Feel free to fork and clone [my repo](http://https://github.com/hilaryml/browser-start-page) to create your own start page and suggest any changes or updates to the original code.

To start, I wrote out a basic list of things that I wanted my start page to address:

1. I wanted it to state the current date and time.
2. I wanted it to greet me like a friendly robot-pet.
3. I wanted it to ask me a provoking yet borderline snooty question - just like a good robot-pet should. (Clearly in my head I was picturing HAL with a British accent and without the murderous tendencies...)
4. I wanted there to be a search bar beneath the question so that I could google-search something cool in response to my hoighty-toighty robot's question.
5. I wanted the whole background to be taken up by a nice photo. Nothing too flashy or distracting. Otherwise I'd end up wasting hours on a travel site instead of doing work...

Onwards!

First I set up a basic index.html file with the skeleton of my start page. I used a basic html form for the search bar and just set the action to "http://www.google.com/search". I left the button out because it looked prettier... Yeah.

```
#index.html
<!DOCTYPE HTML>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Welcome</title>
    <link rel="stylesheet" href="css/style.css">
  </head>
	<body>
    <div class="jumbotron">
      <div class="container">
        <!-- CLOCK -->
				
				<!-- HEADER -->
        <h1>Hello, Hilary.</h1>
				
        <!-- SUB HEAD -->
        <h3>What are you curious about today?</h3>
				
        <!-- SEARCH BAR HERE -->
					<form method="get" action="http://www.google.com/search">
						<input type="text" name="q" size="31" maxlength="255" value="" />
					</form>
					
				</div>
    </div>
  </body>
</html>
```

After ages of fruitlessly searching how to get the date and time to show up, I binge-watched some Netflix and moved on to css styling instead. 

First, I styled the jumbotron div, and provided it with an image to use as my background. I used some handy flexboxes to make sure everything lined up in the centre of the page both vertically and horizontally.

```
.jumbotron {
  background-image: url("../images/IMG_9312_1024.jpg");
  background-repeat: no-repeat;
  background-size: cover;
}

.jumbotron .container {
  color: #ffffff;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
```

Next, I moved on to the header and subhead styles. Because I'm picky, and because I'm easily entertained I spent an embarrassing amount of time playing with colours over [here](http://http://www.color-hex.com/).

![](http://cdn.meme.am/instances/500x/32205253.jpg)

Nuff said.

Anyways here's the css for those bad-boys:

```
h1 {
	font: 600 5em "Times New Roman";
	color: #ffffff;
	padding-bottom: 10px;
	margin: 0 0 5px;
}

h3 {
	font: 200 2em "Times New Roman";
	color: #89a7a7;
	margin: 0 0 5px;
}
```

Totally worth it.

After that I fiddled around with formatting the search box, I didn't want it to be too visible so I played around with the styling before settling on this:

```
input[type="text"] {
  color: rgb(150, 170, 160);
  font: "Times New Roman";
  background: transparent;
  border: none;
  height: 22px;
  width: 500px;
  border-bottom: 1px solid #ccc;
  padding-bottom: 10px;
}
```

This gave me a transparent search box with only a bottom border.

Finishing all of this left me with no choice but to figure out how the heck to incorporate the current date and time. I finally found [this](http://http://www.webdeveloper.com/forum/showthread.php?118292-Adding-current-time-and-date-to-web-site) which saved my butt. I incorporated some of this into my code like so:

```
<!-- CLOCK -->
<div class="clock">
	<script type="text/javascript">
		document.write ('<p>It is currently: <span id="date-time">', new Date().toLocaleString(), '<\/span>.<\/p>')
		if (document.getElementById) onload = function () {
		 setInterval ("document.getElementById ('date-time').firstChild.data = new Date().toLocaleString()", 50)
		}
	</script>
</div>
```

And then styled with CSS:

```
.clock {
  display: flex;
  justify-content: center;
  align-items: center;
  font: 200 2em "Times New Roman";
  color: #759898;
}
```

Awesome! Now to get all of this beautiful code to work its magic you need to go about setting it as your browser's start page. In Chrome, you can do this by navigating to settings and under "On startup", click the "Open a specific page or set of pages" option and then click "Set pages" to open the startup pages box. In the "Enter URL" box, enter "file:///Users/your-username/filepath-to-index.html". Then click 'Ok'.

Next, under the "Appearance" section, click the "Show Home button" option and enter "file:///Users/your-username/filepath-to-index.html" into the space provided.

Now, your brand-spanking-new start page will appear whenever you open Chrome! 

![](http://i.imgur.com/97Nb2vA.jpg)
