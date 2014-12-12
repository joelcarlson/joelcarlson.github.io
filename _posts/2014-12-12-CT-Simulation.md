---
title: "Campep Accreditation Statistics"
author: "Joel Carlson"
layout: page
excerpt: A shiny app for analysis of enrolment and employment for Medical Physics students
---

Here I will describe a fun project I did to simulate a first generation CT scan image using MCNPX.  The idea was to take a simple geometry: a square, a circle, and a triangle, and create an image using backprojection.  The geometry set-up looked like this:

![center](/figs/CTBlog/unnamed-chunk-1-1.png) 


To create an image using backprojection, a projection image must be created by scanning across the geometry.  This projection image is really just a measure of the attenuation between the source and detector over a given interval. The geometry is then rotated and the process repeated. To implement this using MCNPX, each interval represents a unique simulation.  In this case, I used 100 intervals at 36 angles, resulting in 3600 total simulations. Here is a mockup of the (slightly altered) situation at considerably lower resolution for demonstration purposes:

<a href="http://imgur.com/p5aa5nU"><img src="http://i.imgur.com/p5aa5nU.gif" title="Particles" /></a>

In the image, the source is a pencil beam originating below the geometry, the red and blue dots represent photons of 100keV interacting with the geometry. The detector scanning across the image at the top measure the incident flux, which can be interpreted as a surrogate for the attenuation between the source and the detector. 

#Flux Values

The detector outputs a single value at each simulation, which can be plotted against the location of the detector.  Here is what the normalized values looks like at each angle:

<a href="http://imgur.com/baRIvf8"><img src="http://i.imgur.com/baRIvf8.gif" title="source: imgur.com" /></a>

Each of the values is then converted into a pixel value, and a square image is created:

<a href="http://imgur.com/XSqCBy7"><img src="http://i.imgur.com/XSqCBy7.gif" title="source: imgur.com" /></a>

#Image Combination

Next, all of the images are rotated to the angle from which they were obtained:

<a href="http://imgur.com/Q1wTxqu"><img src="http://i.imgur.com/Q1wTxqu.gif" title="source: imgur.com" /></a>

All of the rotated images are then summed together. However, the images must first be darkened such that the maximal sum of pixel values at any given point doesn't exceed 1, the white value of the image processing software used (`EBImage` for R). Summing together the rotated images allows us to see the image being iteratively formed:

<a href="http://imgur.com/yIBS6aU"><img src="http://i.imgur.com/yIBS6aU.gif" title="source: imgur.com" /></a>

Laid side to side the images result in a sinogram:

<a href="http://imgur.com/eFDugnr"><img src="http://i.imgur.com/eFDugnr.png" title="source: imgur.com" /></a>

#Final Image

The last iteration of the image summing gives us the final image:

<a href="http://imgur.com/7lpV0kk"><img src="http://i.imgur.com/7lpV0kk.png" title="source: imgur.com" /></a>

Which looks pretty good! We can even see the how the different densities of the materials changes their visibility in the image!




