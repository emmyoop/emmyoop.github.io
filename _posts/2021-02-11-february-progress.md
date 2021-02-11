---
title: Weather Whiplash February
image: https://live.staticflickr.com/65535/50933099591_d85c894fa6_b.jpg
categories: [life]
tags: []
hidden: false
author: emmyoop
---

So far it's been in the 80s, raining, in the 60s, below freezing and soon it may be snowing.  I'm sure Austin will make it to the 70s again before the month is out.  But still, progress has been made.  

{% include image.html img="https://live.staticflickr.com/65535/50933099641_0139365751_b.jpg" alt="Enchanted Rock and Little Rock" align="right"%}


The family spent Tuesday at Enchanted Rock for a fun filled day of hiking, bouldering and finding Fairy Shrimp in water pools at the peak!  Both kids did great hiking (and listening) and the fresh air was good for us all.  The 2 hours drive to the park was the foggiest it's been in a long time (yet more funky weather) but by the time we hiked to the peak we had blue skies and a light breeze.  Perfect weather for a hike without shade!

Unfortunately my spring garden is in progress, but on hold.  We have snow in the forecast for the **second time** this year.  It's not very common and definitely not conducive to growing delicate seeds and sprouts.  Before this cold snap the kids and I were able to add some compost and peat moss into the garden to start prepping it for planting though.  I'll also be heading to the [Natural Gardener](https://tngaustin.com/) soon to get some seeds.  I'll hold off on their sprouts though - I've gotten them before and they were all oddly full of disease.   

I've also finished up the APIs and Web Scraping course on DataQuest.  I have to say it was pretty basic and just a review of the structure of html and CSS.  It all came back quickly and made sense.  Maybe I'll play with it and scrap some pages for some data.  Next up is the Data Analysis in Business course.

{% include image.html img="https://live.staticflickr.com/65535/50933222617_abaca7fa78_z.jpg" alt="Spot the Station Text Screenshot" caption="Spot The Station Text String" align="right" %}

I've been working on a small project to pull International Space Station (ISS) data based on geographic location on Earth.  NASA has a potentially awesome [Spot the Station](https://spotthestation.nasa.gov/) app where you can enter in your location and it will send you texts when the space station will be flying over that location.  I'm signed up and we often make a night of it with the kids - have a fire in the chiminea, maybe make s'mores, look at the stars and race to see who can see the ISS first.  There are two problems with it.  The UX to sign up for text alerts in painful and confusing.  The other problem is that they send a text that looks like this screenshot.

Click on the html attachment to get the world's tiniest text telling you specifics:

    Time: Sat Feb 06 6:59 PM, Visible: 7 min, Max Height: 58 degrees, Appears: NW, Disappears: SE­Þ

Now, why can the contents of the text not just be the contents of that file?  It is probably an over reaction on my part but I find it frustrating the system works this way.  It motivated me to try to make at least part of it better.  However the API endpoint I'm using does not provide information like max height of direction.  I'm going to look more into the official [NASA APIs](https://api.nasa.gov/) to find a better endpoint to request data.

My project is called [nextSighting](https://github.com/emmyoop/nextSighting).  I set up a small csv file of locations ranging from just a city to an exact address.  I read in that file and convert all the locations to their equivalent latitude and longitude using [geopy](https://geopy.readthedocs.io/en/stable/).  For a bit of extra fun I'm using [folium](https://python-visualization.github.io/folium/) to map all the locations out.  Then I make some API requests to get the next 5 passovers for each location and post the results into a Slack channel using IFTTT.

I've also added some cool tools into my nextSighting project.  I'm using [codeFactor](https://www.codefactor.io) to help write clean code.  I'm also using [Codecov](https://codecov.io/) for, you guess it, code coverage reports.  This is more about getting comfortable with the tools than anything since this is such a small project.  But I've found the best way to start using a new tool is in an environment with little complexity to be able to more easily evaluate any bumps or snags you run across.

I'm using GitHub Actions to hook in to these tools, another new tool for me.  In doing so I've discovered I should be running my project in a Docker container so that I can more directly control what installs in the environment.  I've used Docker a bit but not much.  Yet another hitch to that plan is that Docker doesn't seem to play nice with multiple users on a Mac.  [@Rocktavious](https://twitter.com/rocktavious) and I are sharing the Mac for the days we work and he's using Docker for his work involving Kubernetes.  I'm confident I don't know enough to figure out both the Mac user issue and Docker from scratch.  So I'm putting the GitHub Actions on hold until I can sync up some time for a tutorial with [@Rocktavious](https://twitter.com/rocktavious).

See what I mean about start small to minimize complications?  The need for Docker isn't really related to anything specific in [nextSighting](https://github.com/emmyoop/nextSighting).  It could be easy to start down a rabbit hole of *what's wrong with my project* if my project were more complex.

From here, I have a few things I would like to do to enhance [nextSighting](https://github.com/emmyoop/nextSighting) and consequently future projects.

#### Project Enhancements

- Filter data based on sighting time - no one cares if the ISS passes over during daylight hours (invisible!) or at 1am (can anyone say bedtime?)
- Report weather along with the proposed sighting - if it's going to be overcast the ISS won't be visible
- Incorporate the [folium](https://python-visualization.github.io/folium/) map somehow and send it to slack as well.  Maybe draw the path across the map for each day?
- Pull data from Google Drive, not just a local file
- Find a better API endpoint using [official NASA APIs](https://api.nasa.gov/)
- Of course, build a Docker container to run my project

{% include image.html img="https://live.staticflickr.com/65535/50933332952_8d1b584cb3_b.jpg" alt="Kids Hiking to the Summit" caption="Enchanted Rock Summit Hike" %}
