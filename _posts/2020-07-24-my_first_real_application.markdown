---
layout: post
title:      "my first real application."
date:       2020-07-24 16:07:52 -0400
permalink:  my_first_real_application
---


I wanted this project to be somthing more personal to me. I was diagnosed with MS at the start of the flatiron bootcamp, I was connected with a nonprofit centered around helping newly diagnosed ms patients thrive. One of the things they where dreaming about was an online space they could have medical professionals (educators) post content tailord to their clients needs. Fitting considering I need just such education. So my Aplication is a place where educators can post content to their patients home page and patients could journal about their day to day experiences as well as log their apointment reminders. I found the idea of moving from one train-station to another a helpful analogy when thinking about the controller actions.

An important concept for a controller with restful routes are the naming conventions. Eliminating "extra" words is really important to keep your code simple, readable and clear. Learning to be mindful of the plurality of my controller and building a truely restful controller was dificult. Below is one thing I ran into.

A route to see all the posts requires that we render the index.erb file to the browser. While the code bellow works to present the new post erb file, it is not truely restfull: 
```
get "/post/index" do
      erb :"/post/index"
end
```
the above code is redundant. It gives more information to the user than needed. To refactor you could do somthing like this:
```
get "/posts" do 
     erb:"/post/index"
end
```



this way, the user only sees "/posts". Also notice the pluralization of the route. Its important to have a consistant naming convention both for your own well being and for anyone looking at your code down the line.
Sinatra doesnt rely on pluralization the same way that Rails does. But Its good practice to develop code that is easy to work with. your file name should also be plural when aplicable, controllers for instance:
IE: posts_controller.rb. 
This pluralization is also important in our post.rb model file, lets take a look.
```
class User < ActiveRecord::Base

    has_secure_password
    validates :name, presence: true, uniqueness: true 
    validates :password, presence: true 
    validates :provider_id, presence: true 
    has_many :post 
    has_many :apointment 
    belongs_to :provider 

end

```
here you can see that my has_many relationships are singular! That doesnt make any sense! Ruby is all about beautiful easy to read code.  so lets fix that: 
```
    has_many :posts 
    has_many :apointments 
    belongs_to :provider 
```
isn't that better. Ruby agrees with the changes and we are no longer thown errors. It might seem silly but these patterns are alot more serious when you have 30 controllers and a complex platform. 


The project wet my apatite for web development and im excited to continue! P.S. ActiveRecord is Amazing!
