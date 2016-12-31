---
layout: post
title:  "My Rails Portfolio Project"
date:   2016-12-31 04:10:09 +0000
---


For this project I decided to create a rails web app for a soccer league. The reasoning for this was because I spent the majority of my youth playing competitive rep soccer and was constantly forgetting when and where my practices, games, and tournaments were. Since everything seems to be heading online these days (yay!), I though that this would be a pretty simple and useful way to practice my new Rails skills.

Okay, so to start I created a new repository on github and cloned it on my computer.

To get all of the basics up and running I ran

```
rails install rails
```

and

```
rails new hamilton_soccer_league
```

Next I added some gems to my gem file (omniauth, omniauth-facebook, devise, and pundit) and ran

```
bundle install
```

Next I added the spec files by running

```
rails g rspec:install
```

After all that, I set up my model specs so that I could be sure that I was creating all of my model relationships correctly.

For example:

```
#spec/models/user_spec.rb
require 'rails_helper'

RSpec.describe User, :type => :model do
  let(:team) {
    Team.create(
      :name => "Green Goblins"
    )
  }

  let(:user) {
    User.create(
      :name => "Player 1",
      :email => "email@email.com",
      :password => "password",
      :team_id => team.id
    )
  }

  it "is valid with a team_id" do
    expect(user).to be_valid
  end

  it "belongs to one team" do
    expect(user.team).to eq(team)
  end

  it "#role is 'player' by default" do
    expect(user.role).to eq "player"
  end

  it "#role can be set to :coach" do
    user.role = :coach
    expect(user.role).to eq "coach"
  end

  it "#email returns a string" do
    expect(user.email).to be_a(String)
  end

  it "#name returns a string" do
    expect(user.name).to be_a(String)
  end

end

```

etc...

Next I made my migrations and models (which took a while). And then I worked on getting my models specs to pass. I added some validations to my models to help keep out invalid data.

After that I added a tonne of dummy data to my seed file so that I would be able to play around with my app on the server once I got it up and running.

I built out my routes after that and decided to nest users, games, and practices under teams like so:

```
#routes.rb
...

  resources :teams do
    resources :users
    resources :games
    resources :practices
  end

...

```



At this point I got to work on my controllers and basic views. I installed pundit:

```
rails g pundit:install
```

I wanted to restrict the players’ access to certain parts of the site, so I made it so that only coaches could create teams, games, and practices. In order to do this I used Pundit and then used ‘authorize’ in my controllers to check whether the current_user should be able to complete an action.

```
#app/controllers/teams_controller.rb
…
def create
    @team = Team.new(team_params)
    if @team.valid?
      if authorize @team
        @team.save
        redirect_to team_path(@team), alert: "Team successfully created."
      else
        redirect_to new_team_path, alert: "You are not authorized to complete this action."
      end
    else
      render :new
    end
  end
…

```

Next I create some useful model methods in order to keep logic out of my views. A couple of these included methods that would return a team’s coach’s name as well as the names of team’s players.

```
#app/models/team.rb
…
  def players
    self.users.where(role: 0)
  end

  def coach_name
    self.users.detect { |user| user.coach? }.name
  end
…

```

After that I got to work on the rest of my views. I added a team schedule that would be available for both coaches and users to see.

```
#app/views/teams/schedule.html.erb
<h1><%= @team.name%>'s Schedule</h1>

<% if current_user.coach? %>
  <%= link_to "New Practice", new_team_practice_path(@team.id) %>
  <%= link_to "New Game", new_team_game_path(@team.id) %>
<% end %>

<h4>Practices:</h4>
<table>
  <tr>
    <th><strong>Date</strong></th>
    <th><strong>Time</strong></th>
    <th><strong>Location</strong></th>
  </tr>

    <%= render partial: 'practices/practice', collection: @team.practices, as: :practice %>

</table>

<h4>Games:</h4>
<table>
  <tr>
    <th><strong>Date</strong></th>
    <th><strong>Time</strong></th>
    <th><strong>Location</strong></th>
    <th><strong>Link</strong></th>
  </tr>

    <%= render partial: 'games/game', collection: @team.games, as: :game %>

</table>

```

I used partials to keep things nice and neat.

Then I moved on to making form partials for coaches to create new practices and games. I added in field_with_errors classes to show validation errors.

```
#app/views/practices/_form.html.erb
<%= form_for practice, url: "/teams/#{current_user.team_id}/practices" do |f| %>

  <% if practice.errors.any? %>
    <div id="error_explanation">
      <h2>
        <%= pluralize(practice.errors.count, "error") %>
        prohibited this practice from being saved:
      </h2>

      <ul>
      <% practice.errors.full_messages.each do |msg| %>
        <li><%= msg %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field<%= ' field_with_errors' if practice.errors[:date].any? %>">
    <%= f.label "Date" %>
    <%= f.date_select :date %><br></br>
  </div>

  <div class="field<%= ' field_with_errors' if practice.errors[:time].any? %>">
    <%= f.label "Time" %>
    <%= f.time_select :time %><br></br>
  </div>

  <div class="field<%= ' field_with_errors' if practice.errors[:location].any? %>">
    <%= f.label :location %>
    <%= f.text_field :location %><br></br>
  </div>

  <%= f.submit %>

<% end %>

```

Intense.

After that I added in a nested form for a coach to create a new team as well as all of the players that would be associated with that team. I then created a custom attribute writer for this in the team model.


```
#app/models/team.rb
…
def users_attributes=(users_attributes)
    users_attributes.each do |i, user_attributes|
		    team_user = self.users.build(user_attributes)
        team_user.update(team_id: self.id)
		    self.users << team_user
    end
	end
…

```

Next I decided to work on adding in OmniAuth with Facebook. I ran into problems however, since I wanted users who signed in this way to be able to edit their profiles afterwards to change their names, etc without having to give a password (since they wouldn’t know what had been stored in the database for this, having signed in via Facebook). In order to figure this out I turned to google.

Turns out I had to add the following:

```
#app/controllers/users/registrations_controller.rb
class Users::RegistrationsController < Devise::RegistrationsController

  def update
    @user = User.find(current_user.id)
    if @user.provider == "facebook"
      params[:user].delete(:current_password)
      @user.update_without_password(user_params)
      redirect_to team_user_path(@user.team, @user)
    else
      @user.update_with_password(user_params)
      redirect_to team_user_path(@user.team, @user)
    end
  end

  private

  def user_params
    params.require(:user).permit(:name, :email, :password, :password_confirmation, :team_id, :role)
  end

end


#config/routes.rb
…
devise_for :users, controllers: { omniauth_callbacks: "users/omniauth_callbacks", registrations: 'users/registrations' }
…

```

After all of this I needed to figure out how I wanted to implement scope within my team model. I decided to add a ‘wins’ attribute to my team model which could be edited by that team’s coach after a winning game. Next, I used this to add a couple model methods to my team model that would allow me to filter teams in the teams index by their wins.

```
#app/models/team.rb
…
scope :wins, -> { order(wins: :desc) }
…
def self.top_five_teams
    wins.limit(5)
  end
…

```

I then added the filter button to the team index view.

```
#app/views/teams/index.html.erb
…
<%= form_tag("/teams", method: "get") do %>
  <%= select_tag "team standings", options_for_select(["standings", "top five teams"]), include_blank: true %>
  <%= submit_tag "Filter" %>
<% end %><br></br>
…

```

Lastly, I had to implement my ‘comment’ functionality, which involved allowing users to place comments on game show views, where the comments would act as a join between that game the the comment’s user.

I did this by adding a new comment form to the game show view, as well as using a comment partial to display all of the comment’s for that game.

To check out all of the code for my rails app, click [here](https://github.com/hilaryml/hamilton_soccer_league).


