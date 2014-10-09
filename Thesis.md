---
layout: page
title: ML For Radiotherapy
permalink: /thesis/
---
<p class="message">
  Project page for thesis
</p>

For my thesis project I am looking to use machine learning algorithms and concepts to predict gamma analysis results from fluence maps produced by radiotherapy treatment planning systems.
When a patient is to receive a treatment they must first have several x-ray images taken of them, and based on these images a plan is developed to deliver as much radiation to the tumor
as possible while sparing the surrounding tissues, and any susceptible organs.  This is a complicated process, and the treatment planning system is, at times, over ambitious, creating
a plan so complicated that the linear accelerator cannot physically deliver it.  

To test whether the plan is deliverable, the plannign system calculated what it belives the dose
distribution should look like, then the plan is physically delivered to a measurement device, and the distributions of the two are compared.
Gamma analysis is one way of interpreting the results of the comparison of the distributions.  For example, if the institution wants a gamma passing rate of 90% for 2%/2mm, then 90% of the 2mm by 2mm voxels must be within 2% of each other. 

The process of physically delivering and measuring the distribution is time consuming, and laborious for Medical Physicists.  For my MSc thesis project I am looking to 
create fluence maps based on the calculated dose distributions.  From these fluence maps I hope to extract image features which have predictive power for
gamma analysis results.  From these features I will create a model, most likely using [GBM](http://en.wikipedia.org/wiki/Gradient_boosting) or [Random Forest](http://en.wikipedia.org/wiki/Random_forest) algortithms.

