---
layout: post
title:  "Breaking through mental barriers with Ajax"
date:   2017-01-10 00:39:31 +0000
---


As with anything new you are trying to learn and apply, you tend to hit certain roadblocks that make you stumble (or in my case, really freak-you-the-heck-out). This was the case with me learning how to use Ajax with APIs. I've found the last couple of labs pretty challenging, but writing about the process afterwards has helped me to understand how everything works.

The JS APIs Lab required us to create a basic user interface that allowed the user to create Gists through the GitHub API and then allow the user to view all of their Gists below.

After browsing the links provided I learned that Gists for Github are created via a POST request to /gists, and that the following requirements had to be met:

```
{
  "description": "the description for this gist",
  "public": true,
  "files": {
    "file1.txt": {
      "content": "String file contents"
    }
  }
}

```
[Create a Gist - GitHub](https://developer.github.com/v3/gists/#create-a-gist)

So for the createGist() function that we had to make, the above requirements would be accepted as parameters. And those parameters would in turn be supplied by the user when they fill out the form in the user interface.

I got to work on a basic form in my html file as well as a section that would show the Gists for that user:

```
<div class='form'>
    <label>File Name:</label>
    <input type='text' id='file_name'>

    <label>Content:</label>
    <input type='text' id='content'>

    <label>Description:</label>
    <input type='text' id='description'>

    <label>Token:</label>
    <input type='text' id='token'>

    <a href='#' id='create'>Create Gist</a>
  </div>
	
	<div class='userGists'>
  </div>
```

Next, I went back to my github_client.js file. The file names and parameters that they would accept were already set up for us. So I took a few minutes to try to understand the flow of data in order to figure out where to start first.

I decided to start on the function that would make my first Ajax call to the GitHub API, the createGist() function. I set up my Gist requirements to mirror the format supplied by the website and filled them in with the parameters.

```
var createGist = function(file_name, content, description, token){
  var gist = {
    'description': description,
    'public': true,
    'files': {}
  };

  gist['files'][file_name] = { 'content': content }
```

I initially tried to pass the 'files' both of its parameters at once like so:

```
'files': {
    file_name: {
		    'content': content
		}
}
```

But this didn't work, so I ended up just passing 'files' an empty hash and then passed it the other params after.

Next, I set up my Ajax call to make a POST request to the GitHub API and appended '/gists' to the end of the base url. The lab told us that we needed to add an authorization token like so:

```
headers: { Authorization: token }
```

Once the post request was made, the myGists() function would be called with the response from the ajax call.

```
$.ajax({
    url: 'https://api.github.com/gists',
    type: 'POST',
    dataType: 'json',
    headers: { Authorization: token },
    data: JSON.stringify(gist)
  }).done(function(response) {
    myGists(response.owner.login, token);
  });
};
```

The myGists() function was supposed to query the GitHub API with Ajax in order to return the Gists for that user, using the username and token supplied in the parameters.

After reading more of the supporting documentation, I realized that I needed to write an Ajax call that would make a GET request to /users/:username/gists like so:

```
$.ajax({
    url: 'https://api.github.com/users/' + username + '/gists',
    type: 'GET',
    dataType: 'jsonp'
  })
```
[List a user's gists - GitHub](https://developer.github.com/v3/gists/#list-a-users-gists)

After that I would need to format the response into a list and append the html to the index so the user could view their Gists. The lab stated that we would need to make each list item a link to the Gist and have the link text display the Gist description.

```
}).done(function(response) {

    var html = '<ul>';

    $.each(response.data, function(index, gist) {
      html += '<li><a href=' + gist.html_url + '>' + gist.description + '</a></li>';
    })

    html += '</ul>';
  });

  $('#userGists').append(html);
};
```

Lastly, I needed to fill in my bindCreateButton() function, which would bind the user's click event when they click the "Create Gist" button to the function that will grab the values that the user enters and supply them to the createGist() function:

```
var bindCreateButton = function() {

  $('#create').on('click', function() {
    var file_name = $('#file_name').val();
    var content = $('#content').val();
    var description = $('#description').val();
    var token = $('#token').val();

    createGist(file_name, content, description, token);
  })
};

```

This whole cycle is called in the document ready section like so:

```
$(document).ready(function(){
  bindCreateButton
});
```

My full repo for this lab is [here](https://github.com/hilaryml/js-apis-lab-v-000)
