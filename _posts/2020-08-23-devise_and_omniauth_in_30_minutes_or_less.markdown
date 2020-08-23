---
layout: post
title:      "devise and Omniauth in 30 minutes or less"
date:       2020-08-23 19:51:34 +0000
permalink:  devise_and_omniauth_in_30_minutes_or_less
---


The biggest headache my classmates and I ran into during the rails project was getting devise and omniauth to play well. So, here we go! after generating a new project I suggest getting devise and omniauth set up right away. please note that when you use devise, you are subject to all the cemantics inate with devise. Tred with caution.


# 1. Add gems in your gemfile
```
gem ‘devise’
gem ‘omniauth-github’
gem 'dotenv-rails'
```
adding these dependances will allow us to set up user authentication in a jify

# 2. Run 'bundle install' in your termnial followed by the following commands.
```
rails g devise:install
rails g devise user 
```
now we have stubbed out most of the devise functionality. If you choose edit the users table (leave all the existing fields for devise to work properly)
# 3. Run 'rake db:migrate'
**Awesome, now lets work on omniauth with Devise **

we have already installed the omniauth gem so lets get right into it.
# 5. First we need to update your user model to look something like this:

```
class User < ActiveRecord::Base
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable,
         :omniauthable, :omniauth_providers => [:github]

  def self.from_omniauth(auth)  
    where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
      user.provider = auth.provider
      user.uid = auth.uid
      user.email = auth.info.email
      user.password = Devise.friendly_token[0,20]
    end
  end
end
```
this enables omniauth in the devise settings and sets a user in our database based on the information provided by github.
note that some people have a problem with Github and their email for authentification. In that case, change the user.email line to somthing like this:
```
user.email = auth.info.nickname + "@fun.com"
```
the fun.com is just a place holder.

# 4. In your config/routes.rb file add: 
 ```
devise_for “users, :controllers => { :omniauth_callbacks => “callbacks” }
```
this is the routing for login with github.

# 5. In your terminal run:
```
Rails g controller callbacks
```
# 6. In your callbacks controller lets enter: 
```
class CallbacksController < Devise::OmniauthCallbacksController
  def github
    @user = User.from_omniauth(request.env["omniauth.auth"])
    sign_in_and_redirect @user
  end
end
```
this handles the reentry to your program.
# 7. we are going to take a step back here:
in order to move foreward, we are going to add a provider index and uid index to your users table
Run this code in the terminal

```
rails g migration AddColumnsToUsers provider:index uid:index
```
Dont forget to migrate the file to the database


# 8. Next we need to jump to git hub and set up a new OAuth App token.

Head to your git hub and load your account settings.

At the bottom of your nav bar on the left click 'Developer settings', then 'OAuth Apps', and finaly 'New OAuth App' fill out your application nam, the homepage URL with 'http://localhost:3000/', and your callback Url to 'http://localhost:3000/users/auth/github/callback.'

# One last step!
create .env file in the root directory and enter:
```
CLIENT_IT=enter your info from github here

APP_SECRET=enter your infor from github here
```
Lets add your client_Id and app_secret to your config/initializers/devise.rb file
```
config.omniauth :github, ENV['CLIENT_ID'], ENV['APP_SECRET']
```
finally add the .env file to your gitignore file. (That way nobody has access to your secret information) 

and volia, There you have it.

create a root view and set it as your root in your routes.rb using the following sintax
replace the controller and action with your desired route and launch your app using rails s.
```
root 'controller#action'
```

good luck. 

if you run into any issues be sure to check out the devise and omniauth documentation:

https://github.com/heartcombo/devise
https://github.com/omniauth/omniauth

