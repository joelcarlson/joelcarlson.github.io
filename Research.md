---
layout: page
title: Research
permalink: /research/
---

<p class="message">
While at Seoul National University I have focused my research on two 
different topics: *Machine learning for prediction of MLC positional errors during VMAT* and *Dosimetry for a novel brachytherapy seed*. 
My future research goals include knowledge-based methods for improving disease prognosis, and using these methods for better clinical decision support systems.
</p> 


##Machine Learning to Predict MLC Errors 

![MLC](http://i.imgur.com/UAybAep.png?1)

For my thesis project I have used machine learning algorithms 
and concepts to predict positional errors of multi leaf collimator leaves.

When a patient is to receive a treatment they must first have several 
x-ray images taken of them, and based on these images a plan is 
developed to deliver as much radiation to the tumor as possible while 
sparing the surrounding tissues and any susceptible organs. This is a 
complicated process, and the treatment planning system is occasionally 
over ambitious, creating a plan so complicated that the linear 
accelerator cannot physically deliver it. 

The goal, then, is to correct the plan such that the encoded leaf positions truly represent where the leaves will be.

Reproducible research is important to me, so the progress of this 
project can be tracked at my [Thesis 
Project](http://joelcarlson.github.io/thesis) page. 

Here is a link to the presentation I gave at AAPM 2015 related to this material: [SlideShare link](https://www.slideshare.net/slideshow/embed_code/key/dkzHQwmlWf5ud0)

##Brachytherapy 

Brachytherapy is a process in which small radioactive seeds are placed 
in close proximity to a tumor. Generally, the procedure is performed on 
patients presenting either breast or prostate cancer, however it is not 
necessarily limited to these applications. Currently there are several 
brachytherapy seeds to choose from on the market, however due to 
transportation and decay time issues, it is advantageous for a country 
to produce their own seeds. To this end, the Korean Atomic Energy 
Research Institute ([KAERI](http://www.kaeri.re.kr:8080/english/)) has 
begun production of a novel seed. Before a seed can be used clinically 
it must have several parameters, known as 
[TG-43](https://www.aapm.org/pubs/reports/rpt_84.pdf) parameters, 
physically measured and computationally simulated using Monte Carlo 
methods. 

I carried out Monte Carlo simulations for the new seed using 
[MCNPX](https://mcnp.lanl.gov/) software, and will soon be physically 
measuring the dosimetry parameters using a custom build PMMA jig and 
EBT3 film. More information on this project can be found at my 
[Brachytherapy Project](http://joelcarlson.github.io/brachytherapy) 
page. 


