---
layout: post
title:      "Banjo Market - Sinatra App"
date:       2020-02-21 17:26:57 +0000
permalink:  banjo_market_-_sinatra_app
---


Hi everyone! For my second porfolio project for Flatiron, I decided to extend the theme from my first CLI project: banjos! I set out to build a used banjo marketplace website using Sinatra.

**This is what I wanted my app to do:**

* Create, Read, Update, and Delete (CRUD) both banjos and users
* Allow users to login, edit their profile, and view other user's profiles (and not edit them!)
* When a user account gets deleted, their listings get deleted too
* A non logged in user can only view '/login' page and '/signup' page
* When an error occurs during account creation/editing, display a flash message to the user (e.g. "Your passwords do not match!")
* Have an overall visually pleasing asthetic (a challenge considering this was my first time doing anything front-end!)

I began by building a very basic MVC, to first CRUD users. Once I figured out the routing, I began building CRUD for banjo listings.

My models used a :has_many, :belongs_to relationship. This is where I ran into my first confusing error. For some reason, I wasn't able to connect the users to their listings. When a new listing was created, its user_id was not correlating with the user that created it.

So, after quite a bit of research on Google and Stack Overflow, I found my problem, WHICH WAS SO OBVIOUS. I never created a column in the banjos table for a users_id. And thus, I created a new migration to add a users_id column and my association issue was solved.

Everything after that was smooth-sailing. Once I got all of the backend part of the application written, I moved onto the front-end aspect: HTML, erb, and CSS.

The front-end development was much easier than I expected, considering I had very little experience with it. There are some incredible resources for CSS and HTML help online, and this proved to be very helpful to me. While I created a fairly good looking deskop site, my CSS responsiveness for different screens was not perfect.

Moving into the future, I now have a much better understanding on how to structure HTML to make for easier CSS styling and responsiveness.

**Here are a couple screenshots from a logged in user:**

![](https://i.imgur.com/yyvCj9U.png)

![](https://i.imgur.com/zXsFn3P.png)

![](https://i.imgur.com/3aZ6t9u.png)

![](https://i.imgur.com/zMnVioi.png)

Much like my first porfolio project, beginning this Sinatra project was extremely daunting at first. I did not feel like I understood Sinatra and ActiveRecord enough to be able to build my own app. But, I've found that I learn best when I am building a project of my own and researching the problems and errors that go along with it.

And with that said, if you are feeling nervous about the Sinatra project, YOU CAN DO IT! You will figure it out and you will learn a ton in the process.

Thanks for reading! You can view my project [Github repo here](https://github.com/slaydenriley/banjo_market). Feel free to check it out and run it on your local server.
