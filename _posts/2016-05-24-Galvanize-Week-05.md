---
title: "Galvanize - Week 5"
author: "Joel Carlson"
date: "May 24, 2016"
tags: galvanize
layout: page
excerpt: Week 5 of the Galvanize Data Science Immersive - Intensity ramps up  
---

The intensity of the program really ramped up this week - I have found myself staying later, and working on both sides of my commute. We covered a number of topics this week, and a few of these topics are broad enough to warrant their own full courses, so trying to get a decent grasp on them in the time-span given is, well, hard.

To finish off the week we were given a real-world dataset and simply told to extract business insights from it. The structure of the day was similar to the previous Friday, in which we chose teams and spent the day with the data, finally presenting our results in the late afternoon. Here is a link to our presentation: [link](https://docs.google.com/presentation/d/1Kx9L5QsBF_g2g3e3cVanCA68s6iZ283Q3zWd_bLORQ4/edit#slide=id.g1417cf16ad_0_71)

I very much enjoyed comparing and contrasting this assignment and the previous weeks assignment with my peers in the program. Several of them expressed that they had found the previous case-study more difficult, with the main challenges cited being cleaning the data and dealing with missing values - it was a very messy dataset. Conversely, I found this case-study much more challenging. Interpreting the coefficients, and making sure that our conclusions were statistically accurate was very challenging. The presentations at the end of the day made this very salient to me:  nearly every group expressed different odds ratios for their logistic regressions (which was the obvious choice for an interpretable model in this case). Thus, we all gave slightly different business recommendations. Was having X behavior associated with a 2 times increase in customer turn-over? Or a 5 times increase? or did it decrease turn-over? None of this was ever adequately resolved.

## Day 1

Today we covered a topic that I have experienced first hand - class imbalance. Before coming to Galvanize, in my research, I came across a situation in which my classes were heavily skewed, and did a lot of scrounging around trying to find a good method for handling the issues. Eventually I settled on a class-weighting scheme that I wasn't particularly happy with. In class today I was introduced to a scheme that I like much more than assigning class weights (which was in effect a type of oversampling) - the SMOTE technique. In SMOTE, the goal is to synthesize new data of your under-represented class by randomly creating points similar to existing points. I was pretty excited about the algorithm, and wrote an implementation and explanation [Here]().

## Day 2

Web scraping day!. The instructor emphasized the hacky nature of web scraping, going so far as to refer to it as the wild wild west. We looked at methods for piling website content in a MongoDB, which seemed useful. It is likely that many of the students here will do some sort of web-scraping and NLP for their capstone projects, so this material will figure in heavily.

We also discussed Mongo vs SQL which was very interesting. Our conclusions were that SQL is built for executing simple queries slow, but complex queries very fast. Whereas Mongo executes simple queries fast, but complex much slower. Apparently this is due to the immense redundancy in Mongo. Also, unintuitive for me, there are no joins in Mongo!

## Day 3 and 4

What a whirlwind couple of days. We covered some NLP and some time series - both topics which are deserving of their own entire course. Much left to learn in both topics. Our introduction to NLP was pretty exciting, though! We scraped the entire corpus of New York Times articles using their API and a scraper that we built from scratch. The power of web scraping really came to light today. There is such an immense volume of data out there to be scraped...

## Day 5

I keep on saying this, but this was probably my favorite day of the program. As described in the introduction to this post, we were given a dataset with the goal of extracting actionable business insights. Now here I was planning to do a nice and detailed writeup of the process we took, but one of my peers, Nathan Kiner (who was also one of my team mates for the task), did a much better job than I would have. [Here](http://www.thefutureofself.com/2016/05/case-study-churn-prediction-in-ride-sharing-app/) is his write-up, check it out! If, for whatever reason, you would like to see a more raw version of the analysis, you can see most of what we did [here](https://github.com/joelcarlson/ipython-notebooks/tree/master/Churn_Predictions).
