---
layout: post
title:      "Gigger - A Rails Application for bands and venues"
date:       2020-04-07 18:31:13 +0000
permalink:  gigger_-_a_rails_application_for_bands_and_venues
---


Hi everyone! 

For my Rails portfolio project, I created an app called "Gigger," to help bands and venues book and manage upcoming shows. This project was certainly the biggest challenge for me yet, but I feel like I'm finally getting the hang of rails.

<iframe width="560" height="315" src="https://www.youtube.com/embed/rlJHyWHkSy8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Models

There are 4 main models in my application: User, Band, Venue, and Gig.

## User
```
has_many :band_users
has_many :bands, through: :band_users
has_many :venue_users
has_many :venues, through: :venue_users
has_many :gigs, through: :bands
has_many :genres, through: :bands
```
## Band
```
has_many :band_users
has_many :users, through: :band_users

has_many :gigs, dependent: :destroy
has_many :venues, through: :gigs
belongs_to :genre
```

## Venue

```
has_many :gigs, dependent: :destroy
has_many :bands, through: :gigs

has_many :venue_users
has_many :users, through: :venue_users
```

## Gig
```
belongs_to :band
belongs_to :venue
```
There is also a Genre model, and two many-to-many tables: band_users and venue_users. This is because a user has_many bands and a band has_many users. The same goes with users and venues.

# App overview

There are three main account types a user can make: "band member," "venue manager," and "concert goer." Each account type has different permissions and responsibilities.

* A band member can create a new band, be added to existing bands, edit/delete their own band, create gigs for their band at the venue of their choice, and assign or create new genres.

* A venue manager can create a new venue, be added to an exisiting venue, edit/delete their own venue, and create gigs at their venue.

* A concert goer can view information about bands, venues, and upcoming shows.

* All users can edit/delete their own account, but not others (unless account is an admin).

To manage user abilities, I used the gem "cancancan" to define what each user is permitted to do. Here is how that is managed, in my ability.rb file:

```
class Ability
  include CanCan::Ability

  def initialize(user)
    can :read, :all
    can [:create, :new], User
    if user.present?
      if user.admin?
        can :manage, :all
      # Band Member Authority
      elsif user.account_type == "band_member"
        Band.all.each do |band|
          band.users.each do |owners|
            if owners.id == user.id
              can :manage, band
            end
          end
        end
        can :create, Band
        can :manage, User, id: user.id
        Gig.all.each do |gig|
          gig.band.users.each do |owners|
            if owners.id == user.id
              can :manage, gig
            end
          end
        end
        can :create, Gig
        can :read, :all
        # Venue Manager Authority
      elsif user.account_type == "venue_manager"
        Venue.all.each do |venue|
          venue.users.each do |owners|
            if owners.id == user.id
              can :manage, venue
            end
          end
        end
        Gig.all.each do |gig|
          gig.band.users.each do |owners|
            if owners.id == user.id
              can :manage, gig
            end
          end
        end
        can :create, Gig
        can :create, Venue
        can :manage, User, id: user.id
        can :read, :all
        # Concert Goer Authority
      elsif user.account_type == "concert_goer"
        can :manage, User, id: user.id
      else
        can :read, :all
        can :manage, User, id: user.id
      end
    end
	end
```

This portfolio project definitely pushed me to my limits, and I learned far more than I would have expected in starting it. Feel free to clone my project and check it out! Just use "cd gigger," and "rails s" to start it up. Let me know if you have any questions! Thanks for reading.

[Here is my github repo.](https://github.com/slaydenriley/gigger/blob/master/app/models/gig.rb)

-Riley




