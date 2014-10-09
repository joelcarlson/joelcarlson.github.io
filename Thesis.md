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
as possible while sparing the surrounding tissues and any susceptible organs.  This is a complicated process, and the treatment planning system is occasionally over ambitious, creating
a plan so complicated that the linear accelerator cannot physically deliver it.

To test whether the plan is deliverable, the planning system first calculates what it believes the dose
distribution should look like, and the plan then is physically delivered to a measurement device. It is the job of the medical physicist to compare the measured and calculated distributions.

Gamma analysis is one way of interpreting the results of the comparison.  By way of example, if the institution wants a gamma passing rate of 90% for 2%/2mm, then 90% of the 2mm by 2mm voxels must be within 2% of each other. 

The process of physically delivering and measuring the distribution is time consuming, and laborious for Medical Physicists. Thus to spare valuable clinical resources, for my MSc thesis project I am looking to find a way to *predict* gamma passing rates using only information available in the calculated plan (thus obviating the need for the measurement step in certain cases.)

There has been a certain number of approaches to solving this paper, and several "modulation indices" published.  Most of these have taken into account the acceleration of the MLC leaves, gantry rotation speeds, and number of monitor units delivered per control point.  Recently, medical physicists at Seoul National University Hospital published a novel method of using image analysis techniques (texture analysis - see my [publications](http://joelcarlson.github.io/publications/)) on fluence maps extracted from the calculated plans.  In this paper, six textural features were investigated and found to have considerable correlations with plan deliverability.

It is my hypothesis that there are ways to combine more conventional modulation indices, textural features, and other features from the plans to increase predictive capability. From these features I will create a model, most likely using [GBM](http://en.wikipedia.org/wiki/Gradient_boosting) or [Random Forest](http://en.wikipedia.org/wiki/Random_forest) algorithms.

