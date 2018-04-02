---
layout: post
title:      "Sinatra Project Mode!"
date:       2017-10-26 18:36:34 -0400
permalink:  sinatra_project_mode
---


![](https://i.imgur.com/FFHg9i4.jpg)

A walk at the Park :)



If there is anyone who knows me well and would talk about my habits, the first thing they'd say is that I'm always in the kitchen. So I decided to build something I was very passionate about and knew I was going to be proud to show off to everyone. A recipe creator it is! I know it’s not the most original idea, but I knew that’s what I wanted to make and already had all of these ideas rushing into my head about making it super enjoyable for the user, aka baker. 

I’ve gotta say this project was a lot more mellow than my CLI data gem project. I think that is probably because instead of rushing into it after I had finished the section, I took a couple of days off to decompress and let ideas rush into my head. Your thought process is very different when you're rested.   

Let me tell you about it...

My app is named: Baking Table

It has three models:
* Baker
* Recipe
* Category

Relationships:
* Baker has_many :recipes, has_many :categories through: :recipes
* Recipe belongs_to :baker & category
* Category has_many :recipes, has_many :bakers through: :recipes

Database Migrations
* Baker: username, email, password_digest
* Recipe: name, ingredients, instructions, baker_id, category_id
* Category: name

With this app you can create an account, create your own recipes, edit them, delete them and view them by category. You can also view recipes from other users. I found the biggest challenge for me was working without tests. I had to use Tux quite a bit to try to figure out if my bakers and recipes where generating correctly. It was really cool to get to play around with my objects before I actually put them into production. 

I used these methods to help me with validating if a baker was currently logged in and to help make sure they could only  edit and delete recipes if they had created them.

```
helpers do
    def logged_in?
      !!session[:baker_id]
    end

    def current_baker
      Baker.find(session[:baker_id])
    end
  end

```

Database Takeaway!

`rake db:drop` will drop your database, which means you have to recreate it by running `rake db:create`  and then `rake db:migrate`. I created a seed file and seeded my database with the command `rake db:seed`. 
After dropping my database and trying to recreate it, I kept on running into an error that would tell me my database was not configured in my development file. After much googling and reading StackOverflow, I found I needed to create a database.yml file in my config folder.

```
development:
  adapter: sqlite3
  database: db/development.sqlite3
  pool: 5
  timeout: 5000
	
```

CSS and Styling 
 
Was a total challenge, because I created my app with the `gem corneal` which included a main.css file, I just had to tweak around a few things to get my colors in there, added a new font, a picture and even styled the buttons. This was definitely my favorite part!


The takeaway from this project is that I finally got RESTful routes down. I also had to revisit my CRUD lessons quite a bit to get things working. I realized that rather than trying to build everything at once, it is better to work on one thing at a time, if its broken, troubleshoot and then move on. If you keep skipping around trying to rush the process you'll find yourself in a messy sea of failing controller actions. Happy Coding Everyone!!!! 



