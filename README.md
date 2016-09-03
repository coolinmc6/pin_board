# README

This is a Pinterest-clone following the tutorial by [Mackenzie Child](https://mackenziechild.me/) in his
[12-apps-in-12-weeks](https://mackenziechild.me/12-in-12/) series.  
* The video for the tutorial is located here: [Week 4: How To Build A Pinterest Clone With Rails 4](https://mackenziechild.me/12-in-12/4/) and here: [Youtube](https://www.youtube.com/watch?v=abcnfFS_DS8&index=4&list=PL23ZvcdS3XPLNdRYB_QyomQsShx59tpc-)
* Mackenzie's GitHub repo for this project is here: [pin_board](https://github.com/mackenziechild/pinterest_clone)

####Things to look up later
* 'resources :pins'


####
* There is a pattern evolving in how I build these apps.  Build the model, migrate database, build the controller.  This
sets the foundation.
* Add the index action and then make the routes (resources :pins) and root to index
* build the view for the index
* make the new and create actions
* make the form (as a partial)
* make the show and new views



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






