---
title: "Galvanize - Week 6"
author: "Joel Carlson"
date: "June 3, 2016"
tags: galvanize
layout: page
excerpt: Week 6 of the Galvanize Data Science Immersive - Capstone Ideation
---


This week we explored some clustering techniques, some dimensionality reduction techniques, and then looked at creating recommender systems. We also had our first brain storm session with instructors about capstone project ideas.

I coded up a proof of concept for one of the ideas that I had for a capstone (which I am unlikely to pursue), here is an intermediate result from it:

<img src="https://raw.githubusercontent.com/joelcarlson/joelcarlson.github.io/master/figs/Galvanize/GravityPOC.png"/>

Read on to find out what it is! Or just look at the code [here](https://github.com/joelcarlson/GravityImageFilter).

## Day 1

We looked today at KMeans and hierarchical clustering. These are both relatively simple algorithms, and were very enjoyable to code up. I was rather pleased with my implementation of KMeans, so I cleaned it up and posted it [here](https://github.com/joelcarlson/ipython-notebooks/tree/master/KMeans_Implementation).

## Day 2

Dimensionality reduction techniques - we very briefly reviewed and discussed previous methods we've used, including the lasso for regressions, and stepwise regrsession (both forward and backwards). Some subsequent reading on feature selection makes me very weary of using stepwise regression (although I must admit that the idea of stepwise regression is very appealing/intuitive to me, statistical validity be damned!). There are a large number of references for why we should avoid stepwise, so here are a few (in order of how much I enjoyed reading them): [1](http://andrewgelman.com/2014/06/02/hate-stepwise-regression/), [2](http://www.stata.com/support/faqs/statistics/stepwise-regression-problems/), and [3](http://stats.stackexchange.com/questions/7935/what-are-disadvantages-of-using-the-lasso-for-variable-selection-for-regression).

Why do we do feature reduction in the first place?
 - Remove multicollinearity and redundant features
 - Remove sparsity (mitigating curse of dimensionality)
 - Allow interpretation and visualization
 - Reduce computational load
 - Discovery! Exploration!

Certainly good reasons for dropping some features. With the above reasons motivating us, we looked at more sophisticated techniques, such as principal components analysis (which I previously used to help reduce a huge number of image features in a classification pipeline, see [here](http://joelcarlson.me/research/glioma/)), and singular value decomposition.

### Day 3, 4, 5

These three days were spent understanding the foundations of recommender systems, then understanding a commercial implementation of a recommender system using [Dato Graphlab Create](https://dato.com/products/create/) (actually, this was the first piece of commercial software that we have used in the entire course!), and finally, creating our own recommender system using a huge database of jokes, and their ratings by users (which have been being collected for the past 15 years!).

### Capstone Project Meeting

Although this was not a particularly long, temporally speaking, part of the week, I found it to be highly impactful. We met with instructors in groups of four to present the capstone ideas that have been buzzing around in our heads over the past several weeks. It seems everyone wants to use convolutional neural networks to classify images. This was a little surprising, given that we have haven't actually studied neural nets in the class thus far. It is a little less surprising, though, having seen the rpevious cohort's capstone presentations - lots of CNNs.

I presented several ideas, one of which I was quite excited about doing, but am now leaning away from. It was something like this:

#### Problem statement:

I have so many photos, all of which are fairly drab, and a little washed out. I want to be able to edit them automatically.

#### Proposed solution:

There are millions of high quality photos out there, why not use them to color my own photos.

#### Proposed method:

Scrape photos from a number of different sources and find some quality metric to keep only images that are of "good quality" (suitable metric yet to be defined). I hypothesize that, for a given image category, there exist a small number of color palettes that humans find appealing ("representative palettes", if you will). For example, you can imagine that for a category such as sunset photos, there may be a palette devoted to capturing the yellows, reds, and oranges of those vibrant, fiery sunsets. There may be a further palette that captures the blues, purples, and greens of those more kaleidoscopic evenings. I would extract these palettes from images after categorizing the images using (gasp!) a CNN that is pre-trained on something like imagenet.

With a number of palettes in hand for a number of different image categories, I would cluster them within each image category to acquire the "representative palettes". The end-user would be presented with an input box for their image, which would classify the image, automatigically choose the most similar representative palette, and then shift the colors from their image to more closely match the palette.

To do the shift, I would use an image filtering idea that I've been tossing around for a while. I've typically described it as analogous to gravity - within some color space (RGB, HSV, whatever) each color can be measured as a distance away from any other color. For each pixel in the image, compute the distance to each color in the palette, and pull it towards each color weight by the inverse (or squared inverse) of the distance, and multiply by some constant weighting factor. In this way, colors similar to those in the palette get closer to those in "good quality photos", while those further away remain untouched. Fun!

I coded up a proof of concept for the filtration part of the algorithm, [take a look at the code here](https://github.com/joelcarlson/GravityImageFilter)!

#### Why I don't want to do it anymore:

I think that this project would be a lot of fun to code up, but perhaps not so impressive as a data science product. It's more programming than data. Furthermore, there is no well defined quality for success. Who is to say whether the images that I created are better than those put in? It's all subjective. I suppose a researcher could find the answer using an mturk survey, but that's not really the point of the capstone.

#### Conclusions:

I have a few other ideas, but this is getting a little long, I think I will discuss them in next week's post. See you then!
