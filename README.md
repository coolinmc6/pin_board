# README

This is a Pinterest-clone following the tutorial by [Mackenzie Child](https://mackenziechild.me/) in his
[12-apps-in-12-weeks](https://mackenziechild.me/12-in-12/) series.  
* The video for the tutorial is located here: [Week 4: How To Build A Pinterest Clone With Rails 4](https://mackenziechild.me/12-in-12/4/) and here: [Youtube](https://www.youtube.com/watch?v=abcnfFS_DS8&index=4&list=PL23ZvcdS3XPLNdRYB_QyomQsShx59tpc-)
* Mackenzie's GitHub repo for this project is here: [pin_board](https://github.com/mackenziechild/pinterest_clone)

####Things to look up later
* 'resources :pins'
* What is ":index" at the end of the generate migration command?
* panels in Boostrap and how to use them
* the member do syntax in routes.rb after adding the like button


#### CM Comments
* There is a pattern evolving in how I build these apps.  Build the model, migrate database, build the controller.  This
sets the foundation.
* Add the index action and then make the routes (resources :pins) and root to index
* build the view for the index
* make the new and create actions
* make the form (as a partial)
* make the show and new views
* use Bootstrap as a basic styling until you can actually get in there and make it look good
* after you add a feature, you go back to the view to see how you actually use it...it's a constant cycle of looking at 
the model, the controller, the view, etc.



####5:20
* We added three gems: HAML, bootstrap-sass and simple_forms
* initialized the git repo and updated the README
* and we're off......

####6:00
* generate Pin model and Pins controller
```ruby
rails g model Pin title:string description:text
rails db:migrate
rails g controller Pins
```
* Create a blank index action and update config/routes.rb file:
```ruby
# pins_controller.rb
def index
end

# routes.rb
Rails.application.routes.draw do
  resources :pins
  root "pins#index"
end
```
* get the no view error because we haven't created an index action yet
* create the following:
  * index.html.haml
  * new.html.haml
  * _form.html.haml

####11:18
* simple_form gem instructions:
```shell
rails generate simple_form:install --bootstrap
```

####~18:00
* He creates a find_pin method that is private and only available to some of the actions.
```ruby
# pins_controller.rb
before_action :find_pin, only: [:show, :edit, :update, :destroy]
.
.
.
def find_pin
	@pin = Pin.find(params[:id])
end
```
####~25:00
* I was having a problem with the confirmation window popping up multiple times when I wanted to delete an item.  I
edited the config/environments/development.rb file:
```ruby
# the default was true
config.assets.debug = false
```
* While installing Devise, I grabbed the wrong version from the rubygems site.  Remember to grab right version!
* The Devise steps:
```ruby
gem 'devise', '~> 4.2'
```
```shell
bundle install
rails generate devise:install
```
```ruby
# environments.rb => just add at the bottom
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```
* root is already set
* alerts/notices are already set
```shell
rails g devise:views
rails g devise User
rails db:migrate
```
* After adding Devise, we need to associate pins and users.  Users will have many pins and pins will belong to users.
But writing the necessary syntax in each model is not enough, we have to create a migration:
```shell
rails g migration add_user_id_to_pins user_id:integer:index
```
* This is the resulting migration:
```ruby
class AddUserIdToPins < ActiveRecord::Migration[5.0]
  def change
    add_column :pins, :user_id, :integer
    add_index :pins, :user_id
  end
end
```
* What is ":index" at the end of the generate migration command?
* Add Bootstrap.  Look at the application.html.haml file to see this:
```haml
%nav.navbar.navbar-default
  .container
    .navbar-brand= link_to "Pin Board", root_path

    - if user_signed_in?
      %ul.nav.navbar-nav.navbar-right
        %li= link_to "New Pin", new_pin_path
        %li= link_to "Account", edit_user_registration_path
        %li= link_to "Sign Out", destroy_user_session_path, method: :delete
    - else
      %ul.nav.navbar-nav.navbar-right
        %li= link_to "Sign Up", new_user_registration_path
        %li= link_to "Sign In", new_user_session_path
.container
  - flash.each do |name, msg|
    = content_tag :div, msg, class: "alert alert-info"
  = yield
```
* Look at how Bootstrap navbar's work...I need to remember this
* Notice the "-" that is used to run ruby code using the user_signed_in? method.  Everything appears to be very
self-explanatory but talk it out so that you understand it
* We then wrap the flash messages and the yield in a container to provide that nice padding

####~42:20
* Add paperclip gem
* Following steps to add necessary info to pin model (pin.rb).
* Add migration:
```shell
rails generate paperclip pin image
rails db:migrate
```
* Add a line in your form where a user can select an image
* Update the 'pin_params' in the pins controller to actually allow users to upload images
* The line below, although in haml, is from the docs for the paperclip gem under View Helpers
```haml
= image_tag @pin.image.url(:medium)
```
* After completing the image uploading functionality, I edited many of the views so that the user could actually
see the image they uploaded!  Nothing is really groundbreaking but I did want to point out a few things:
  * Use common sense!  When they are editing a tag, doesn't it make sense that they'd actually want to SEE what 
  pin they are updating!  Adding that little image isn't difficult so don't get hung up on trying to memorize the
  exact code that is used...it's all in the docs!
  * In the index view, doesn't it make sense that a user can click on the image and be taken to that pin?  Again, 
  use common sense, this isn't too difficult, you are simply using the 'link_to' functionality and putting the image
  inside that...instead of text, there is an image!
####50:00
* Committed recent changes (ability to upload images)
* I was not able to get the transitions working but I'll take a look later

####Last 30 minutes
* Added the ability to like items and brought in the heart glyphicon.  To do that, we used the acts_as_votable gem.
One part that I still need to learn is the modifications to the routes.rb file...I don't quite get this yet:
```ruby
resources :pins do
  member do
    put "like", to: "pins#upvote"
  end
end
```
* The last 20 minutes or so of the video was touching up the styling...some of which I did, some that I didn't.
* I was unable to get the masonry working but as I said above, I will have to play with that later.
* The app is complete



