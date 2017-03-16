---
layout: post
title:  "Verbatim - My Final Project"
date:   2017-03-16 05:42:17 +0000
---

I’d had a couple ideas for my angularjs project bouncing around in my head, but wasn’t completely sure which one I wanted to implement. After spending a week or so playing around (freaking out) with my basic rails-angular set-up, going through the trial and error process, and making plenty of mistakes (but learning a whole bunch in the process), I decided to move forward with Verbatim.

Verbatim is an app designed to allow users to create stories collaboratively, with each user only able to contribute up to 250 characters at a time.

I liked the idea of this app because of its collaborative aspect. I think it’d be pretty cool to be able to log in and see how a story you’ve contributed to has progressed as other users have made their mark on it.

To start, I created an empty repository on github. Next I ran `rails install rails` and `rails new verbatim` which set-up most of my files.

Next I added some gems to my gem file and ran `bundle install`.

```
gem 'active_model_serializers'
gem 'angularjs-rails'
gem 'angular-rails-templates'
gem 'bower-rails'
gem 'angular_rails_csrf'
gem 'rspec-rails'
```

After that I created my routes and used rails generators to create my migrations, models, and controllers.

```
#config/routes.rb
Rails.application.routes.draw do
  namespace :api do
    resources :users
    resources :posts
    resources :sessions
    resources :post_users
  end

  root 'application#index'
  get '*path' => 'application#index'
end

```

Then I installed bower by running `rails g bower_rails:initialize json`

This created a bower.json file and a config/initializers/bower_rails.rb file. I added any angular dependencies I needed into the bower.json file:

```
{
  "vendor": {
    "name": "bower-rails generated vendor assets",
    "dependencies": {
      "angular": "latest",
      "angular-ui-router": "latest",
      "angular-messages": "latest",
      "angular-resource": "latest"
    }
  }
}
```

And ran `bower install`.

Next I made the action for my root page (application#index) and added ng-app, and the ui-view directive to the application/layout.

Once I got that set-up, I added a home state to my app.js file, and a home template.

I hit a snag when setting up my sign in and sign up pages as I forgot to include the csrf stuff.

Thanks to Luke's Angular flatcast, I figured out what I was missing:

```
angular.module('app', ['templates', 'ui.router', 'ngMessages', 'ngCookies'])

  .config(['$httpProvider','$stateProvider', '$urlRouterProvider', function($httpProvider, $stateProvider, $urlRouterProvider) {
    $httpProvider.defaults.headers.common['X-CSRF-Token'] = $('meta[name=csrf-token]').attr('content')

...
```

Next I worked on the rest of my user states. For this I used a User Service that communicated with the app’s back-end. I ran into problems saving user info between states since the $scope kept changing, so I added $cookies to my User Service and used one to store the info for the current user, removing the cookie when the user signed out.

```
angular
  .module('app')
  .service('UserService', ['$http', '$cookies', function ($http, $cookies) {

    var currentUser;
		
		return {
		  ...
      setCookie,
      getCookie,
      removeCookie
    }
		
		...
		
    function setCookie(user) {
      currentUser = user;
      $cookies.putObject('currentUser', user);
      return currentUser;
    }

    function getCookie() {
      currentUser = $cookies.getObject('currentUser');
      return currentUser;
    }

    function removeCookie() {
      $cookies.remove('currentUser');
    }
}])
```

Then I moved on to posts. I ran into problems trying to get the many-to-many relationship between users and posts working with angular for some reason. To solve this, I ended up adding a PostUserService, as well as a create route and action for the post_user join table. I then used the PostUserService to create new instances of the join table and save them in the database. I called on this Service every time I created or updated a post in the posts_controller. I had to be careful with how I coded the addPost() and updatePost() function in the posts_controller, as the sql queries they resulted in would sometimes fire at the same time, and one would fail.

```
angular
  .module('app')
  .factory('PostUserService', ['$http', function ($http) {

    return {
      addPostUser
    }

    function addPostUser(postId, userId) {
      const request = {
        method: 'POST',
        url: '/api/post_users',
        headers: {
            'Content-Type': 'application/json'
        },
        data: {
          post_id: postId,
          user_id: userId
        }
      }

      return $http(request)
        .then(response => response.data)
        .catch(error => console.log(error))
    }

  }])
```

Added search filters for searching posts on post index, posts on user show page, and maybe users on user index.

```
<!-- templates/posts/index.html -->

<input
  class="search"
  ng-model="ctrl.search"
  ng-model-options="{ debounce: 1000 }"
  placeholder="search stories" />
```

Next I added seeds to my database so that I could play around with it a bit and check for bugs (there were many).

I used ng-if and state filters in order to add logic to my navbar so that it wasn’t displaying unless a user was signed in.

```
<div ng-if="'home.users' | isState">
  <ng-include src="'angular/templates/application/_navbar.html'"></ng-include>
</div>
```

I also had to add logic so that only logged in users had access to the app, after trying a bunch of different tactics, I ended up using the UserService and $cookies to the posts and users controllers.

```
if (UserService.getCookie() === undefined) {
  if ($state.is('home.users') || $state.is('home.profile') || $state.is('home.user')) {
    $state.go('home')
  }
}
```

After all that, I added in bootstrap and started making things pretty.

![Verbatim](http://imgur.com/a/JGMbs)

I'm very happy with how the project turned out. I always learn the most from these assessment projects, and this one was no exception.

The full code for my project can be found [here](https://github.com/hilaryml/verbatim).
 
