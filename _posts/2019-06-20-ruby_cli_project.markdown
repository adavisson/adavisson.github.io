---
layout: post
title:      "Ruby CLI Project"
date:       2019-06-20 14:11:41 +0000
permalink:  ruby_cli_project
---

I thought this project turned out to be quite interesting, and it really provided me a lot of confidence. I was a little afraid that I was going to stumble a lot more than I did, but it really was just a validation of everything I have learned so far.
	
My original idea was to scrape a web page of a local brewery to provide information on each one of the beers they provide, but it turned out that the web page was going to be pretty difficult to scrape. I then decided to go with the Top 50 rated beers on the web site Untappd.

I ended up using 6 classes. One class contained a lot of common methods for a few of my classes to inherit. The CLI class handles all of the presentation and user interaction. The Scraper class handles the job of scraping the web page, and I also implemented the logic of object creation in this class as well with the help of some functions from other classes. The Beer class is the most important to this project. It contains all of the information about each beer such as name, brewery, style, etc. I also implemented a Brewery class and a Style class. Both of these classes are small, but were very helpful in organizing the information and presenting it back to the user.

Overall the project went pretty smoothly. One issue I ran into was that some of the information used the same class in the tags, but fortunately it was all uniform and the information was all laid out in the same order. Another tedious thing was trying to get all of the spacing right so that the information got presented to the user cleanly.
