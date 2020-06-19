---
layout: post
title:      "My first project, a battle of witts."
date:       2020-06-19 20:11:45 +0000
permalink:  my_first_project_a_battle_of_witts
---



There I was, week three of the programThere I was, week three of the program, feeling confident and competent. it only took about 20 minutes into the cli project to realize that much like the first mile, I was way in over my head. Once I got my project working I decided to spend some time reviewing my experience. In this blog I want to share some things that i've learned about planning, process and coding during this insane week.


Planning... Flatiron is kind enough to provide a lot of planning resources and examples. Use them. It sounds simple right? but seriously when I started my project I felt like I had learned about each element independently and that I was left to figure out how to bring them together on my own. Use the resources... They really do help you to get a birds eye view.

Planning continued... Get organized!!! Break the project down into buckets and figure out what belongs in each. Then sketch out when you might tackle each component of your program. This was so important to me, it seemed like a mountain in my mind, but surely one step at a time even mountains are small. Take the time to write out your schedule and draw a flow diagram. Then in your editor you can set up the classes that you will need, and iron out your dependencies. 

Time to code...or is it? Talk through the program with a rubber duck, friend or classmate. Pay attention to how the information you are pulling has to move from class to class, change, be sorted and presented in the cli. Once you've done this, sketch out the methods that you think you will need in each class. By this time, you have a solid idea of how the CLI is meant to look, how the methods to manipulate the data need to look, and how your doing your API call. All that is left is to slap a few lines of code into each method and see what happens.


Tips and Tricks!! One of the coolest code snippets i've learned about so far is how to do a mass assignment. My api had a lot of data and I wanted each one to be an object I could call and manipulate. This code snippet is an example of a mass assignment. Each park that I initialized contained over 15 objects that I wanted to store. But like most rubyists I am too lazy to enter each one. This brilliant little line of code saves each nested hash as an object for use later! I wish I could say that I came up with this on my own, but coupled with this trick is a great tip: ask for help and talk through your code with classmates.
```
def initialize(args)
     args.each {|key, value| self.send("#{key}=", value)}
end
```      

With this Mountain of a month behind me, im excited for mod #2and implement what I learned about the development process.
blessings on your day and happy coding!








