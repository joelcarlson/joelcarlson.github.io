---
title: "The Complexity of MLC Movement"
author: "Joel Carlson"
layout: page
excerpt: A few .gifs of the complex movements of MLC leaves
---

## MLCs?

When a modern treatment plan is delivered using the Volumetric Modulated Arc Therapy (VMAT) technique, one of the ways the x-ray beam is modulated is through the use of a Multi-Leaf Collimator (MLC).  The MLC is a set of metal bars, called leaves, which travel back and forth in front of the beam, blocking it in certain areas and letting it pass in others. Here is an image of a typical MLC:

<img src="http://i.imgur.com/onD7Z1R.jpg" title="A Typical MLC" />

MLCs themselves aren't particularly attractive to look at, however, their movements are extremely complex and interesting to watch. In the movements you can see the general shape of the tumor being irradiated, and watch how the beam is made to evade any organs or other healthy tissues.

## Prostate Plan

I had the opportunity to analyse a set of MLC positions for a few treatment plans and made some .gifs out of them. Here is a relatively simple prostate VMAT plan:

<img src="/figs/MLCMovement/prostateGIF.gif" title="A relatively simple prostate plan" />

The image consists of one data point per MLC leaf per control point.  There are 120 MLC leaves, and 356 control points per plan, meaning this image contains 42,720 points!

The animation moves much faster than the real MLC.  During actual delivery the linear accelerator takes ~0.42s for every 2 degrees, but the gif takes only ~0.09s. The abrupt shift halfway through is when then linear accelerator has to stop and switch directions after the first arc.

## H&N Plans

Head and Neck plans are significantly more complicated, and generally utilize many more of the leaves than do prostate plans. Here are two different head and neck plans:

<img src="/figs/MLCMovement/hn1GIF.gif" title="A typical HN plan" />

<img src="/figs/MLCMovement/hn2GIF.gif" title="A more complex HN plan" />

## Errors Between Planned and Delivered Positions

Radiotherapy plans are complicated, there is no doubt about that. It is important, then, to realize that they must be delivered by physical components. The above gifs display the planned positions of the MLC, but can the physical MLC actually achieve those positions?

The following gif shows errors between planned and delivered positions. Red for errors which occur when the leaf is left too far into the center of the MLC, and grey when it is too far outside. These are precisely the types of errors my research works to avoid!

<img src="/figs/MLCMovement/planVsDeliverGIF.gif" title="Differences between planned and delivered positions" />

That's all for now!
