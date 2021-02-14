---
layout: post
title:      "My Portfolio Website | Rails/React+Redux"
date:       2021-02-14 19:06:40 +0000
permalink:  my_portfolio_website_rails_react_redux
---


Hi everyone! Well, this is finally it. After a year and a half of hacking away at Flatiron's online self-paced software engineering course, I've finished my last project. The crux of the course. A "magnum opus." The most frustrating, bug-ridden $%#$@#....well, nevermind. I DID IT!

For my final project, I decided to build something that would help kickstart my career in web development. Using a Rails API backend and React + Redux on the frontend, I created my very own portfolio website to showcase my previous work at Flatiron, my blog posts, and my future work as a developer.

That's right, my portfolio project is a portfolio. 🤯

## Video Walkthrough

I'll add a link here when I get to the walkthrough tonight, Feb 14th 2021...

## Rails API Overview
My backend is fairly simple. I have 5 tables: users, posts, comments, tags, and posts_tags (a join table to create a belongs_to_and_has_many relationship between posts and tags). While the primary user on this application is me (as an admin), people are able to signup on my website to post comments on my projects and blog. Here is my schema.db file:

```
create_table "comments", force: :cascade do |t|
    t.text "content"
    t.integer "post_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.integer "user_id"
  end

  create_table "posts", force: :cascade do |t|
    t.string "title"
    t.text "content"
    t.integer "user_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.string "category"
    t.string "image_link"
  end

  create_table "posts_tags", id: false, force: :cascade do |t|
    t.bigint "post_id", null: false
    t.bigint "tag_id", null: false
    t.index ["post_id", "tag_id"], name: "index_posts_tags_on_post_id_and_tag_id"
  end

  create_table "tags", force: :cascade do |t|
    t.string "name"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  create_table "users", force: :cascade do |t|
    t.string "name"
    t.string "email"
    t.text "description"
    t.string "password_digest"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.boolean "admin", default: false
  end
```

I've been comfortable working with Rails for a while now, so the backend was a breeze to set up! Once I got all of my associations working, controllers, and models built, the only maintaince I had to do throughout this project was update my JSON serializers to render the information I needed on the frontend.

## Frontend Overview (ReactJS + Redux)
My frontend using React + Redux was a HUGE learning experience for me. While I breezed through the React and Redux sections in the course, I never really quite understood complex they were until I began coding my project. The first hurdle I had to get over was the project structure using containers, components, actions, and reducers. It took me a long time to understand the "flow" and be comfortable implementing new features. 

As I coded along, my project became riddled with files and I had to condense the project into even MORE folders. Holy cow, talk about a lot to keep track of. While I love the idea behind Redux, it surely makes you code a ton to even build the simplest of features. 

From the basic user, my frontend looks simple and minimalistic. I have a home page, project page, post page, and resume page. In addition to this, there are signup and login components for users wanting to comment on my posts.

From the admin side, the site gets a lot more complex. Admins on my website have access to a full dashboard, which includes a post/project builder, editor, user accounts manager, and tag manager. The "backend of the frontend" is a fully operational CMS (content management system) for myself, much like Wordpress!

While I won't go into specific details of my containers, components, actions, and reducers (feel free to scour my code [here](http://github.com/slaydenriley/portfolio)), I do want to point out one feature on the frontend that I'm very proud of: my **rich text editor.**

## Rich Text Editor using Quill
Because I was creating a CMS for myself, I wanted to be able to *style* and *design* my blog posts the way I wanted them to appear on the web, just like you would do with Wordpress or Medium. So, I began researching how this could be possible. I came across "Rich Text Editors," which essentially convert styled content into HTML that can be saved as a string to the database. Upon render, the HTML string is converted back into straight HTML and displayed on the page. Cool!

I found that the Quill Editor would best suit my needs, so I went ahead and installed it with npm. After some configuration and troubleshooting, I got what I accomplished, a fully functioning rich text editor!

![image](https://i.imgur.com/ItKpiAp.png)

And there we go! As a published post, it is styled exactly how I did it in my editor.

![](https://i.imgur.com/ecWTmEc.png)

## Conclusion
I've learned so much at my time at Flatiron and I'm incredibly excited to continue my education as I transition into a new career. This portfolio project was at times frustrating, rewarding, complicated, and enjoyable, but overall I am so happy to be comfortable with React and Redux now, and I can't wait to dive even deeper into those technologies.

Please feel free to check out my code [here.](http://github.com/slaydenriley/portfolio)

If you're working through your React/Redux project, or any other project at Flatiron for that matter, feel free to reach out if you have any questions or need some motivation.

-Riley Slayden


