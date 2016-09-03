# README

This is a recipe application following the tutorial by [Mackenzie Child](https://mackenziechild.me/) in his
[12-apps-in-12-weeks](https://mackenziechild.me/12-in-12/) series.  
* The video for the tutorial is located here: [Week 4: How To Build A Pinterest Clone With Rails 4](https://mackenziechild.me/12-in-12/4/) and here: [Youtube](https://www.youtube.com/watch?v=abcnfFS_DS8&index=4&list=PL23ZvcdS3XPLNdRYB_QyomQsShx59tpc-)
* Mackenzie's GitHub repo for this project is here: [pin_board](https://github.com/mackenziechild/pinterest_clone)

```ruby

```

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









