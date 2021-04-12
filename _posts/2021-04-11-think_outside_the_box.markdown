---
layout: post
title:      "Think outside the box"
date:       2021-04-12 03:16:55 +0000
permalink:  think_outside_the_box
---


For my final project, I wanted to do something with sports betting.  I've played around with sports betting programming for years using PHP on a host service that I use for other web projects.  I already had a pretty solid understanding about the concepts of sports betting and with an existing foundation to "Reduxify", it seems like it would be a nice learning opportunity.  I could focus on honing my knowledge of React / Redux without having to complicate matters by also having to learn about sports betting - which is, in itself, complicated enough.  

The problem is ... sports scores and odds are time critical.  Every day you have different games in multiple leagues.  Games have start and end times and scores change through the course of the game until the final score is reached.  You have to have an outside source to supply the data.  The source that I had used for the last few years was a rudimentary scrape of a page that, over time, was no longer as easily scraped.  

I wanted to look for an actual API anyway so I began Googling for sports scores and odds API's and found an API that I wanted to use.  It is a "fee for use" API and I really didn't need to incur any unnecessary costs just for the purposes of this project.  Since the API had a free level as long as the calls to the API did not exceed a certain number within 24 hours, I decided I would build a "middleware" API using my knowledge of PHP that would limit my number of API calls.  I used a MySQL database to keep track of the number of calls within a certain time period as well as recording and then providing the response output from the source API.  My PHP application would use the count to control whether to provide the live API source response or serve the response from the source stored in the database.  I would allow the live response output no more than once per hour which was more than suitable for my development and testing purposes.  

I had to get over the feeling of somehow cheating the API provider of their due charges for their use but I also knew that there was no need to incur costs for the repeated (occasionally unintentional) calls to the source API that occur during development, for the sole purpose of having an external source to build on.  If, for some reason my project would be somehow monetized in the future, I would be happily grateful to pay an external source for their information.  

So, my point in discussing this is to help someone "think outside the box" .  When considering using an external API, if you see limitations to what API you choose, you may be able to find a way to remove or mitigate those limitations which would then allow you to develop your application to achieve it's greatest potential.
