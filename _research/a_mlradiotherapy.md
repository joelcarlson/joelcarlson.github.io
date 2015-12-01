---
title: "Machine Learning to Improve Radiotherapy Accuracy"
author: "Joel Carlson"
layout: pagewithjs
permalink: /research/mlradiotherapy/
thumbnail: "/figs/ResearchImages/MLCfilter.png"
excerpt: "Decision tree regression techniques for predicting and correcting for errors in radiotherapy plans."
---

Here is the presentation I gave at [AAPM 2015](http://www.aapm.org/meetings/2015AM/) regarding this work:

<iframe src="https://www.slideshare.net/slideshow/embed_code/key/dkzHQwmlWf5ud0" width="476" height="400" frameborder="0" marginwidth="500" marginheight="0" scrolling="no" style="float:right"></iframe>


Radiotherapy plans have become increasingly complex in recent years, and with this
 added complexity have come numerous new sources of errors during plan delivery.
 One such source of errors is the multi-leaf collimator (MLC), the final component
 between the linear accelerator and patient. Tasked with shaping the radiation beam
 in real-time during delivery, the movement of the MLC must be highly precise. However,
 due to the highly choreographed nature of MLC movements in modern plans, the beam shapes become
 slightly deformed, leading to plans not being delivered exactly as intended.

In this project, I used decision-tree regression techniques to predict MLC errors,
 and incorporated them into the treatment planning system. 
 This research allows treatment planners to view a more accurate representation
 of the dose as it will truly be delivered to the patient.

Click [here](http://joelcarlson.me/2015/11/14/mlc-dashboard/) to view a web-interface which allows access to the models developed in this work.

#Introduction

Before a radiotherapy plan is delivered to a patient, the ability of a treatment unit
to deliver to treatment as intended must be verified. To test this, a treatment planning system is
used to calculate the dose distribution of the plan on a CT scan of a verification unit (rather than 
a scan of the patient). Then, a physical linear accelerator is used to deliver the plan
to the same verification unit. The calculated dose, and the delivered dose are then compared.
If they match closely, then the plan is verified. If there are large discrepancies then actions must be taken 
to decrease the complexity of the plan. This is a time consuming process, and it is therefore advantageous
for the treatment planning system to calculate the dose distribution as accurately as possible. Thus,
 in this project I used machine learning techniques to improve the calculation accuracy
 by taking into account the uncertainty in MLC positions.

#Background

It is important to understand some of the important components and quantities within the
treatment verification process.

###Multi-leaf Collimators

As stated above, the MLC is the final component between the linear accelerator and the 
patient. It is used to shape the beam into the complex shapes required. The MLC used in this
study is known as the Varian Millennium 120 MLC, as shown here:

<a href="/figs/mlradiotherapy/millenniumMLC.PNG" data-lightbox="MLC" data-title="The Varian Millennium 120 MLC"><img src="/figs/mlradiotherapy/millenniumMLC.PNG" alt="Millennium 120 MLC" /></a>

The MLC consists of 120 individual leaves, 60 per side. Each side consists of 40 inner leaves of 5 mm width, and 20 outer (10 per side) 10 mm leaves. 
The movement of such an MLC is visualized [here](http://joelcarlson.me/2014/12/29/MLC-Movements/). During delivery of a plan, 
the MLC reports it's real time position in the form of "motor counts", which can be converted to millimeters using a manufacturer
specified and medical physicist verified constant of proportionality.

It has been hypothesized that the MLC is the main contributor of errors which cause plans to fail the verification comparison.

Accounting for differences in planned and delivered MLC positions is likely to be important for increasing the accuracy
of dose calculation. The goal, then, is to use any information we can extract from the MLC during delivery to predict
errors in positions, and then feed these back to the treatment planning system, so that it may calculate the 
distributions with knowledge of how the MLC behaves in the real world.

###Gamma Analysis

Gamma analysis is one way of interpreting the results of the comparison between planned 
and delivered dose distributions. It is widely used within radiotherapy, but this comparison is
an active area of research. 

This method was put forth in 1998 by Low et al (1998) and further described in Low and 
Dempsey (2003). Consider two dose 
distributions, a reference and an evaluated distribution. The Gamma at 
each point is given by 

<a href="/figs/mlradiotherapy/gammanalysis.png" data-lightbox="Gamma" data-title="Gamma formula"><img src="/figs/mlradiotherapy/gammanalysis.png" /></a>

<br>The delta terms are the DTA and dose difference criteria. The other 
terms: 

<a href="/figs/mlradiotherapy/gammanalysisterms.png" data-lightbox="Gamma" data-title="Gamma formula terms"><img src="/figs/mlradiotherapy/gammanalysisterms.png" /></a>

Are the distance and dose difference between the reference and evaluated 
points. D<sub>r</sub>(r<sub>r</sub>) and D<sub>e</sub>(r<sub>e</sub>) 
are the reference and evaluated doses at points r<sub>r</sub> and 
r<sub>e</sub>. Gamma is found for each point in the evaluated 
distribution, and the smallest of these values becomes the gamma at that 
reference point, this is repeated for each reference point. 

A point is said to pass gamma if the gamma value at that point is less 
than one. For gamma analysis a plan for which passing rate remains above 
\\(90\%\\) with strict tolerance, such as \\(2\%/2 mm\\) can be considered 
dosimetrically robust. 

#Methods

The goal is to predict errors in MLC positions before the plan is delivered for verification.
Thus, we need to extract features from the MLC which may lead to said errors, and are available
 before the plan is first delivered. 

All plans in this study were volumetric modulated arc therapy (VMAT) plans. During a VMAT plan,
the head of the linac rotates around the patient, constantly delivering radiation. Internally, the 
plan consist of a number of "control points", or angles, at which the positions for each leaf of the MLC are defined. 
Since there are 120 MLC leaves, and a conventional VMAT plan has 356 control points, we have 
42,720 leaf positions to work with. We also know the time between control points, as the head of the linac
rotates at a constant speed. Finally, we know the angular separation of the control points is 2.0341 degrees.

###Feature extraction

With the above information, we can extract a number of features which may offer predictive ability. For each leaf we can 
extract the velocity at each control point of each lea




