---
title: "Galvanize - Week 03"
author: "Joel Carlson"
date: "May 10, 2016"
tags: galvanize
layout: page
excerpt: Week 3 of the Galvanize Data Science Immersive - Righting my wrongs with regressions  
---



This week we progressed to doing much more sophisticated exploratory data analysis and plotting, and through this I have decided that seaborn is a saving grace for matplotlib. I am still feeling as though matplotlib is a huge step backwards, but seaborn does much to offset that feeling (and the plotting style of seaborn has even been endorsed by [Hadley](http://www.github.com/hadley) himself, so who am I to complain). In coming to this conclusion, I identified within myself a certain resistance to learning and understanding matplotlib syntax which I must be mindful of moving forward - that sort of attitude serves no one, least of all myself.

Having now been here for about 3 weeks, my 'routine' is beginning to feel a little more set - for those readers who may be considering a similar experience, this is how it looks. Keep in mind that your results would obviously vary, given that I am commuting to SF from Palo Alto. Most of the students seem to have found living arrangements in SF, although I know of several others in my cohort in a similar position to me.

Time  |  
------------- | -------------
6:45  | Wakeup!
7:00  | Leave apt. to catch 7:19 (ideal) or 7:26 (less ideal) Caltrain
7:19  | Spend some time reviewing for the coming classes
8:30  | Mini-quiz/warmup available, work on it until class
9:00  | Class 1
10:15 | Begin individual project, third coffee
12:00 | Lunch "break", but everyone works through it anyways
13:00 | Class 2
14:15  | Begin working on pair programming assignment
19:ish | Catch a Caltrain back to Palo Alto
21:ish | Home! Workout, dinner, relax, review
24:ish | Attempt to get to sleep at a halfway decent time

Prospective boot-campers take note: Given this schedule it is difficult to maintain good work habits, a social life, and healthy amounts of sleep. Choosing [two of those is much more do-able](http://imgur.com/EZl3a81); I chose to sacrifice sleep, but again, your mileage may vary.

## Day 1

Linear regression from a matrix and linear algebra perspective. We spent some time working through the famous iris dataset, and even played with the cars dataset, both of which I am very familiar with - they are very common teaching tools in R. It was nice and familiar to see them, as it reminded me of my first exposures as I was going through the Johns Hopkins data science specialization on Coursera; ah nostalgia!

We went ahead and derived the linear regression closed-form solution today using matrix notation, which leads to a very tidy equation. Deriving linear regression is relatively straight forward, and reveals a nice explanation of where the coefficients in a linear regression model come from, and how they transform the data points. Going through the derivation is highly recommended, and an excellent source is from page 43 to 49 of [Elements of Statistical Learning](http://statweb.stanford.edu/~tibs/ElemStatLearn/)

## Day 2

We again spent the day on linear regression, but this time from a much more qualitative, inference based perspective. Model building and trouble shooting were focused on pretty heavily, as were the interpretation of coefficients in univariate and multivariate models, along with the interpretation of interaction terms (for binary variables, at least).

I must admit, I was glad to spend some time with coefficient interpretation. I got myself all tongue-tied last week when I was asked about how to interpret the interaction coefficient in a model my girlfriend had built - I stumbled through a technically incorrect explanation, and felt more than a little embarrassed once the dust had settled ("shouldn't you know this stuff?" - "yes, yes I should...").

Inspired by the days work, I wrote a side blog post related to exploring and visualizing a simple interaction, you can see that post [here]()

## Day 3

This morning we had the opportunity to watch the final project presentations of the previous cohort of students. There was large diversity among the projects, with some focusing on clustering and classification (e.g. clustering similar cars by image), others focused on interpretation to gain insight from data (e.g. understanding the genre of music lyrics, linear model building for F1 tire wear), and others consisting largely of exploratory analysis. All in all I was fairly impressed with the calibre of projects, and in particular the front facing sides of the projects - nearly all of them were wrapped in user friendly web interfaces. I enjoy the thought that there is this algorithmic heavy lifting going on behind the scenes which is all abstracted away from the user.

The remainder of the day was spent going over cross-validation schemes (e.g. leave-one-out, k-fold, etc), and then on regularized regression (ridge and lasso). The presentation of regularization was very academic, and I've made a mental note to myself to attempt to gain an intuitive understanding of how they work, why they work, and how to interpret the coefficients (look out fora blog post covering this specifically in the future). Also on the list for building intuition: eigenvalues and eigenvectors. Given the already sparse free-time available to me, it could be a while until I get to that. We do have a week off coming up in a month, but by then I imagine I will have a pile of other concepts to hammer out.

## Day 4

A two-hour assessment to start the day off. It was tense going in to the classroom this morning as everyone was gearing up for our first assessment in 1.5 weeks. The topics covered stretched back to themes we covered in the first week. On the coding side, there was some pandas and some unexpectedly advanced numpy, alongside model building in sklearn and implementation of a t-test. There was also a bayesian A/B testing question which involved running a simulation that sampled from beta distributions based on clicks and views on a website, and a probability simulation. The 'written' section of the test included assessments model quality, and qualitative descriptions of how to set up a frequentist hypothesis test for determining the mean number of users on a website, and a qualitative description of the bias/variance trade-off.

They day's schedule was thrown off by the lengthy assessment, and this resulted in a much more casual afternoon pairs sprint than is typical. To be truthful, it was nice to feel as though there was a little more time to dig in and understand the project well, rather than rushing and hacking together solutions to complete the work in a reasonable timeframe.

Logistic regression was the afternoon focus, which followed naturally from our work with linear regression. Coefficient interpretation for logistic regression is, for me however, much less intuitive than for the linear case. In linear regression, given a variable X with coefficient $\beta$, you get to say "A one unit change in X corresponds to a $\beta$ unit change in the response variable, holding all other variables constant".

For logistic regression, the coefficients represent the *log odds*, so to be even remotely natural, they must be transformed to the 'odds ratio' by exponentiating them. Once you have taken the exponential, given a variable X with coefficent $\beta$ you can then say "A one unit change in X corresponds to a $\beta$ increase in the odds of Y occurring" (with all other variables held constant).

## Day 5

Today was one of the best days I have had within the program. The topic was gradient descent, which is a very nice way of optimizing a function. It is used to optimize the coefficients of models subject to some cost function. That sounds a little abstract, but imagine that you have some model, and there exists some combination of parameters for the model (for example, regression coefficients) that minimize, say, the sum of squares. That is, our goal is to find the coefficients which, when plugged into the model, minimize the sum of squared distances to our line. Given that we know the function we are trying to minimize (the RSS), we first initialize our coefficients to some random value, commonly 0, and take the gradient of the cost function (the derivative in each direction). The gradient will give us the direction of the greatest change, so we increment our coefficients and take the derivative again. In this way, we continue traveling down the gradient until we reach some local minima (here lies an issue with gradient descent: it is not possible to know if your minima is the global minima).

It is a fantastic technique, and the discussions following its presentation was fantastic. I felt that the instructor was very invested in helping us to understand (he continued taking questions more than 30 minutes after the end of class). What I enjoyed the most, however, was the assignment. We were tasked with building, from scratch, a non-generalized form of gradient descent (non-generalized because we were given the function to minimize, rather than providing the function as an argument to our algorithm). It made me happy to see everyone in the class with their notepads out, doing the calculus, running through fake data to confirm calculations...Good fun!
