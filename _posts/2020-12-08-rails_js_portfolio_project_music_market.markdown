---
layout: post
title:      "Rails + JS Portfolio Project: Music Market"
date:       2020-12-09 02:32:31 +0000
permalink:  rails_js_portfolio_project_music_market
---


For my Rails API and Javascript portfolio project, I created an app called "Music Market" which allows users to sell their used instruments to other users. Thus far, all of my Flatiron portfolio projects have employed a music theme, so I had to continue on with this project. :)

Before I go over the details of Music Market, let me first say that this was the most challenging project I've had to create at Flatiron. For whatever reason, it took me MONTHS to wrap my head around Javascript, especially after working for so long with Rails. While there are many similarities between Rails and Javascript, the small differences in syntax between the two completely threw me off. That along with dealing with multiple COVID quarantines and job switches in my normal life, this project has been a long time coming and I'm so happy to finally be finishing it.

Here is a video overview of my project:


## Rails API Overview

My Rails API backend was fairly simple to set up. I have 2 main resources: ***User and Listing.***

A User:

```has_many :listings```

A Listing:

```belongs_to :user```

Each controller renders serialized JSON to the frontend fetch requests, and can *Create, Read, Update, and Delete*. I also used a *Session Controller*, which helps with the login process and renders errors if something goes wrong:

```
class SessionsController < ApplicationController
    def create
        user = User.find_by(username: params[:username])

        if user && user.authenticate(params[:password])
            session[:user_id] = user.id
            render json: user, only: [:username, :name, :id]
        elsif user == nil
          payload = {
            error: "Cannot find user! Please check username.",
            status: 400
          }
          render :json => payload, :status => :bad_request
        elsif user.authenticate(params[:password]) == false
          payload = {
            error: "Incorrect password. Please check password.",
            status: 400
          }
          render :json => payload, :status => :bad_request
        end
    end

    def destroy
        if session[:user_id]
            reset_session
        end
    end
end
```

That's the basic overview of my Rails API! It was simple for me to setup and provides the basic structure required to support a JS frontend.

## Vanilla Javascript Frontend Overview

As I stated before, JS was very difficult for me to learn. I found, however, that the more I worked on this project, the clearer it became to me.

I first started coding my frontend in a single file, *index.js*. However, this QUICKLY filled up with code and became incredibly confusing to read and navigate. I began thinking how I could break it up to separate my concerns. Coming from Rails, I really enjoyed using a MVC (model, view, controller) structure. Therefore, I broke up my code like this:

```
Frontend
    Fetchers
	     listing_fetcher.js
	     session_fetcher.js
		   user_fetcher.js
		
    Views
		   app.js
		   listing.js
		   users.js
```		 
			 
As you can see, I separated my code between Fetchers and Views, listings and users. This helped me a TON and kept my code neat and clean. Each of these files used Javascript classes as well, so everything was nicely organized and easy to call upon from other files.

Here is an example of some fetch code:

```
// // USER FETCHER COMMUNICATES WITH RAILS API FOR USES //
class UserFetcher {
  constructor() {
    this.fetchUsers();
  };

  fetchUsers() {
    fetch(`${BACKEND_URL}/users`)
    .then(users => users.json())
    .then(users => Users.renderUsers(users))
  };

  static fetchSingleUser(id) {
    fetch(`${BACKEND_URL}/users/${id}`)
    .then(user => user.json())
    .then(user => Users.renderSingleUser(user))
  };

// PATCH REQUEST TO EDIT USER //
static editUser() {
  let name = document.querySelector("#edit_name").value;
  let username = document.querySelector("#edit_username").value;
  let description = document.querySelector("#edit_description").value;
  let email = document.querySelector("#edit_email").value;
  let password = document.querySelector("#edit_password").value;
  let password_confirmation = document.querySelector("#edit_password_confirm").value;
  let id = current_user_id

  let formData = {
    name: name,
    username: username,
    email: email,
    description: description,
    password: password,
    password_confirmation: password_confirmation,
    id: id
  };

  let configObj = {
     method: "PATCH",
     headers: {
         "Content-Type": "application/json",
         "Accept": "application/json"
       },
       body: JSON.stringify(formData)
    };

    fetch(`${BACKEND_URL}/users`, configObj)
    .then(user => user.json())
    .then(function(user) {
      if (user.status === 400) {
        let error_message = document.querySelector(".edit_user_error_message")
        error_message.innerHTML = ""
        error_message.innerHTML += `<p class="error_message">${user.error}</p>`
        loginForm.style.display = "block"
      }
      else {
        Users.renderSingleUser(user)
      };
    });

};
};
```

And here is an example of some view code:

```
// USER INSTANCE CREATED WHEN LOGGED IN FROM APP CLASS //
class Users {
  constructor() {
    this.listeners();
  };

// LISTENS FOR BUTTON CLICKS //
  listeners() {
    users.addEventListener("click", function() {
      let user = new UserFetcher
      App.removeActiveButton()
      users.classList.add("active_button")
    });
    account.addEventListener("click", function() {
      UserFetcher.fetchSingleUser(current_user_id)
      App.removeActiveButton()
      account.classList.add("active_button")
    });
  };

// RENDERS ALL USERS //
  static renderUsers(users) {
    App.clearMain();
    allUsers.style.display = "block";

    users.forEach(user => {
      let newHtml = `
        <button class="user_card" id="${user.id}">
        <h4>${user.name}</h4>
        <p>${user.username}</p>
        <p>${user.email}</p></button>`;

      allUsers.innerHTML += newHtml
    });

    Users.userCards();
  };

// RENDERS SINGLE USER //
  static renderSingleUser(user) {
    App.clearMain();
    singleUser.style.display = "block";

    // This function takes a user's listings and iterates through them //
    function renderListings() {
      let listings = user.listings
      listings.forEach(listing => {
        let html = `<p><a href="#" class="user_listings" id=${listing.id}>${listing.title}</a></p>`
        singleUser.innerHTML += html
      });
      Users.userListingsAccount()
    };

    // General HTML to be used for all user accounts //
    let newHtml = `
      <h3>${user.name}</h3>
      <p><em>Username: ${user.username}</em></p>
      <p><em><a href="${user.email}">Send email</a></em></p>
      <h4>${user.name}'s Listings </br>`

    // Adds an edit button to account if user is logged in //
    if (current_user_id === user.id) {
      singleUser.innerHTML += newHtml + `<button class="edit_account_button" id=${user.id}>Edit Account</button>`
      renderListings()
      Users.editUserButton(user)
    }
    else {
      singleUser.innerHTML += newHtml
    };
  };

// MAKES USER ACCOUNTS CLICKABLE //
  static userCards() {
    let card = document.querySelectorAll(".user_card")
    card.forEach(button => {
      button.addEventListener("click", function() {
        UserFetcher.fetchSingleUser(`${button.id}`);
      });
    });
  };

  static userListingsAccount() {
    let listings = document.querySelectorAll(".user_listings")
    listings.forEach(listing => {
      listing.addEventListener("click", function () {
        ListingFetcher.fetchSingleListing(`${listing.id}`)
        App.removeActiveButton()
        listings_button.classList.add("active_button")
      })
    })
  }

  static editUserButton(user) {
    let button = document.querySelector(".edit_account_button")
    //let button_submit = document.querySelector(".edit_listing_submit")

    button.addEventListener("click", function() {
      let name = document.querySelector("#edit_name");
      let username = document.querySelector("#edit_username");
      let description = document.querySelector("#edit_description");
      let email = document.querySelector("#edit_email")
      let password = document.querySelector("#edit_password")
      let password_confirmation = document.querySelector("#edit_password_confirm")

      name.value = `${user.name}`;
      username.value = `${user.username}`;
      description.value = `${user.description}`;
      email.value = `${user.email}`;

      App.clearMain();
      userEditForm.style.display = "block";
    });

    edit_submit.addEventListener("click", function() {
      let editUser = UserFetcher.editUser(`${user.id}`);
    });
  };
};
```

Ultimately, separting everything helped me complete this project sane and also helped further my understanding of Javscript Object Orientation and how it can be used to keep things grouped together.

## Conclusion

Thanks for taking the time to read this post! Feel free to browse my source code here: https://github.com/slaydenriley/music_market.





