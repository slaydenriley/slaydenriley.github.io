---
layout: post
title:      "CLI Portfolio Project, stelling_banjos"
date:       2019-12-20 11:35:23 -0500
permalink:  cli_portfolio_project
---

While coding consumes many hours of my day, my true "day job" is not as a student at Flatiron. It is, rather, as a professional banjo player. Throughout the year, I travel around the country with my band "National Park Radio" and play music for thousands of people at festivals, breweries, bars, houses, parks...you name it.

That being said, I am OBSESSED with banjos. Specifically, Stelling banjos, which I believe are the best banjos ever built. So for my CLI Data Gem porfolio project, I thought it would be fun to create a "Stelling Banjo Catelog"  which people could use to view all the Stelling banjos for sale on elderly.com, get the price, and optionally view more information about each banjo.

# Building My *stelling_banjos* Gem

Before beginning this project, I was incredibly nervous. I felt like I did not understand enough about OO Ruby to actually build a gem of my own. However, the Music CLI final project and the Student Scraper proved to me that I COULD do these things, and after completing them I got a burst of confidence.

For the stelling_banjos gem, I used three classes, all under the **StellingBanjos** module: a **Scraper Class**, which scrapes data from elderly.com using Nokogiri; a **Banjo Class**, which actually creates the banjos from scraped info from the Scraper Class; and finally a **CLI Class** which ran the CLI and styled it to be visually pleasing to the user.

The program works like this:

1. The executable ("../bin/banjo") starts the CLI class by calling *StellingBanjos::Cli.new.start*

2. If the user decides to enter the catelog, the Scraper Class uses the provided link to scrape the webpage and put information into class variable, *@@all.*

3. At this point, the Banjo Class creates banjos through a class method, *create_from_catelog*, which takes in the hash from Scraper.all

4. The CLI now neatly prints the banjos into a catelog format, as seen here:
```
Loading...

                   ~ Supplied by www.elderly.com ~
    1. Stelling Staghorn with Old Wood Rim & Case, SN7307 - $6,250
    2. Stelling Master Flower (1995) - $3,600
    3. Stelling Bellflower (1984) - $2,700
    4. Stelling Crusader (2006) - $3,275
    5. Stelling Swallowtail with Old Wood Rim & Case - $4,600
    6. Stelling Sunflower with Old Wood Rim & Case - $3,900
    7. Stelling Crusader with Old Wood Rim & Case - $3,900
    8. Stelling Bellflower (1977) - $2,750
    9. Stelling Sunflower (1988) - $2,850
    10. Stelling Bellflower with Old Wood Rim & Case - $3,900 - SOLD OUT
    11. Stelling Sunflower (2013) - $3,200 - SOLD OUT
    12. Stelling Red Fox (2002) - $2,950 - SOLD OUT
    13. Stelling Carolinian Deluxe (1989) - $5,500 - SOLD OUT
    14. Stelling Master Flower with Old Wood Rim & Case - $4,500 - SOLD OUT
    15. Stelling Red Fox with Old Wood Rim & Case - $3,900 - SOLD OUT
    16. Stelling Staghorn with Old Wood Rim & Case, SN7297 - $6,250 - SOLD OUT
    17. Stelling Staghorn with Old Wood Rim & Case - $6,630 - SOLD OUT
    MENU
    ══════════════════════════════════════════
    -Enter a banjo number for more information
    -Type 'Exit' to exit
    ══════════════════════════════════════════
```

5. The user can then enter a banjo number for more information, or leave the program
6. If they choose more information, the input number retrieves the link provided as an instance of the banjo object, and hands it over to the Scraper class to scrape the detailed information page
7. This is then displayed neatly to the user through the CLI Class, like this:

```
3
    Loading...
        █ STELLING BELLFLOWER (1984) - $2,700
        █ A fine resonator banjo, this Stelling Bellflower has a walnut neck and
        resonator. The ~3/4" maple rim is equipped with an 11” synthetic head
        and a Stelling Wedge-Fit tone ring. Other features include pearl flower
        & leaf fingerboard inlays, a one-piece flange, and ivoroid binding throughout.
        This banjo is in very good+ condition.
        █ Interested in buying? Go here:
        ==> https://www.elderly.com/collections/stelling/products/stelling-bellflower-1984
    MENU
    ══════════════════════════════════════════
    -Enter another banjo number
    -Type 'Catelog' to view the catelog again
    -Type 'Exit' to exit
    ══════════════════════════════════════════
```


8. If the banjo is sold out, instead of displaying a link going to elderly.com, a google link will be displayed so the user can search google for that specific banjo instead:

```
10
    Loading...
        █ STELLING BELLFLOWER WITH OLD WOOD RIM & CASE - $3,900
        █ Handcrafted in Heards, Virginia, this professional-level banjo showcases
        the natural beauty of unstained Virginia black walnut.
        █ SORRY! This banjo is SOLD OUT from elderly.com! Let's check Google instead...
        ==> https://www.google.com/search?q=stelling-bellflower-with-old-wood-rim-&-case
    MENU
    ══════════════════════════════════════════
    -Enter another banjo number
    -Type 'Catelog' to view the catelog again
    -Type 'Exit' to exit
    ══════════════════════════════════════════
```



Originally, I had it so the Scraper Class would scrape *both* the catelog and the more information page for EVERY banjo at the start of the program. This however caused an extremely slow load time. I refactored the code so that the detailed information page was only scraped if the user wanted that information.

So there it is! While I feel like my vocabulary is still lacking a bit with OO Ruby, I do *finally* have a clear understanding on how classes interact with each other and the differences between instance/class methods.

Overall, I am really proud of myself for building this gem. 2 weeks ago, I never would have imagined being able to do this. For anyone worried about their first porfolio project, keep your head high, search google a LOT, and don't give up! It'll make sense eventually.

If you want to check out my public repo, you can do so here: [https://github.com/slaydenriley/stelling_banjos](http://). Or if you want to install my gem and run it in your terminal (super easy!), simply type "gem install stelling_banjos", and type "banjo" into your terminal to start the CLI.

Thanks for reading,
Riley













