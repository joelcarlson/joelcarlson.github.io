---
layout: page
title: Research
permalink: /research/
---
<p class="message">
  Occasionally I perform research..
</p>

While at Seoul National University I have focused my research on two different topics: *Dosimetry for a novel brachytherapy seed*, and *Machine learning to predict gamma passing rates from linear accelerator fluence maps*.

##Brachytherapy

Brachytherapy is a process in which small radioactive seeds are placed in close proximity to a tumor.  Generally, the procedure is performed on patients presenting either breast or prostate cancer, however it is not necessarily limited to these applications.
Currently there are several brachytherapy seeds to choose from on the market, however due to transportation and decay time issues, it is advantageous for a country to produce their own seeds. To this end, the Korean Atomic Energy Research Institute ([KAERI](http://www.kaeri.re.kr:8080/english/)) has begun production of a novel seed.
Before a seed can be used clinically it must have several parameters, known as [TG-43](https://www.aapm.org/pubs/reports/rpt_84.pdf) parameters, both measured and simulated using Monte Carlo methods.

The Monte Carlo simulations for the seed were carried out using the [MCNPX](https://mcnp.lanl.gov/) software.  More information on this project can be found at my [Brachytherapy Project](http://joelcarlson.github.io) page.

##Machine Learning and Fluence Maps

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

Reproducible research is important to me, so the progress of this project can be tracked at my [Thesis Project](http://joelcarlson.github.io) page.
