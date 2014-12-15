---
title: "CT Simulation"
author: "Joel Carlson"
layout: page
excerpt: A fun project simulating the creation of a backprojection CT image using MCNPX
---

Here I will describe a fun project I did to simulate a first generation CT scan image using MCNPX.  The idea was to take a simple geometry: a square, a circle, and a triangle, and create an image using backprojection.  The geometry set-up looked like this:

![center](/figs/CTBlog/unnamed-chunk-1-1.png) 


To create an image using backprojection, a projection image must be created by scanning across the geometry.  This projection image is really just a measure of the attenuation between the source and detector over a given interval. The geometry is then rotated and the process repeated. To implement this using MCNPX, each interval represents a unique simulation.  In this case, I used 100 intervals at 36 angles, resulting in 3600 total simulations. Here is a mockup of the (slightly altered) situation at considerably lower resolution for demonstration purposes:

<img src="http://i.imgur.com/p5aa5nU.gif" title="Particles" />

In the image, the source is a pencil beam originating below the geometry, the red and blue dots represent photons of 100keV interacting with the geometry. The detector scanning across the image at the top measures the incident flux, which can be interpreted as a surrogate for the attenuation between the source and the detector. 

#Flux Values

The detector outputs a single value at each simulation, which can be plotted against the location of the detector.  Here is what the normalized values looks like at each angle:

<img src="http://i.imgur.com/baRIvf8.gif" title="Tally Values" />

Each of the values is then converted into a pixel value, and a square image is created:

<img src="http://i.imgur.com/XSqCBy7.gif" title="Projection Images" />

Laid side to side the images result in a sinogram:

<img src="http://i.imgur.com/7y6ShrR.png" title="Sinogram" />

#Image Combination

Next, all of the images are rotated to the angle from which they were obtained:

<img src="http://i.imgur.com/Q1wTxqu.gif" title="Rotations" />

All of the rotated images are then summed together. However, the images must first be darkened such that the maximal sum of pixel values at any given point doesn't exceed 1, the white value of the image processing software used (`EBImage` for R). Summing together the rotated images allows us to see the image being iteratively formed:

<img src="http://i.imgur.com/yIBS6aU.gif" title="Backprojection Image Formation" />

#Reconstructed Image

The last iteration of the image summing gives us the backprojection reconstructed image:

<img src="http://i.imgur.com/7lpV0kk.png" title="Backprojection Image!" />

Which looks pretty good! We can even see the how the different densities of the materials changes their visibility in the image! However, the image has a sort of radial blur due to the nature of backprojection.  That is, each point in the 'true' image is reconstructed as a circular region with decreasing intensity as distance from the center increases.

#Filtered Backprojection

To solve the blurring issue the image must be filtered.  This is done before reconstruction (ie. summing of the rotated images).  The filter kernel used is approximately the Sinc function, but with even indices equal to zero.  The derivation of the kernel can be found <a href="http://www.dspguide.com/ch25/5.htm">here</a>. Here is a graphical representation of the convolution kernel:

<img src="http://i.imgur.com/R8Vy3Fu.png" title="Sinc(ish) Kernel" />

By performing a convolution of the tally values and the kernel we get the following:

<img src="http://i.imgur.com/Tlf4Owj.png" title="Convolution" />

From this we can see that the "baseline" value has been converted to be approximately 0.5, which corresponds to grey in the final image.  Also of note is the emphasis and subsequent de-emphasis of the "edges" in the tally values.

The image reconstructed with this convolution kernel looks like this:

<img src="http://i.imgur.com/f1KOdL7.png" title="Filtered image" />

#Dynamic Range Adjustment

I found this image to be lacking in dynamic range. A histogram of the normalized pixel values after convolution shows that there are some outliers which squeeze all the other values into a very small range of values.

<img src="http://i.imgur.com/uI7J0vs.png" title="Histrogram with outliers" />

To remedy this I took a very unsophisticated solution: If the value was greater than 0.8 I set it to 0.8, and if it was less than 0.3 then it was set to 0.3.

I then renormalized the convolution values, and the resulting convolution values look like this:

<img src="http://i.imgur.com/jpJFM9b.png" title="Adjusted convolution" />

Varying together, the adjusted and unadjusted tally values:

<img src="http://i.imgur.com/eMxsPaG.gif" title="Convolutions" />

The sinogram of the filtered images:

<img src="http://i.imgur.com/QmZ4eJg.png" title="Filtered Sinogram" />

#Final Image

The filtration results in the final image, which is significantly less blurry than the original backprojection, and has more contrast than the original filtered image (although more artifacts):

<img src="http://i.imgur.com/zPcGqMO.gif" title="Filtered Animation" />

<img src="http://i.imgur.com/aBYyrQ7.png" title="Final Image" />




