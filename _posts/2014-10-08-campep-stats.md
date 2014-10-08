---
title: "Campep Accreditation Statistics"
author: "Joel Carlson"
layout: page
excerpt: A shiny app for analysis of enrolment and employment for Medical Physics students
---

To practice as a Medical Physicist in North America one has to become certified by the ABR.  As of 2010, only those who have graduated from a CAMPEP accredited Medical Physics program are eligible to take the ABR exams, and thus be able to pursue the practice. 

Something nice that has come from this is that CAMPEP requires schools to post several statistics pertaining to enrolment and employment of their students.  However, there are no regulations for *how* this data is to be presented, resulting in a situation where each school posts it's stats in a different way.  Some post a .pdf on their homepage, others bury it wth grad school admission statistics, others accessible using only the skeleton key provided to the dean.

I wanted to get an overview of these statistics, and to do so I combed through all of the schools on the accredited schools [list](http://www.campep.org/campeplstgrad.asp) and compiled it here, with a convenient [**Shiny**](http://shiny.rstudio.com/) app to allow some data exploration.  Here is the app:

<iframe src="https://joelcarlson.shinyapps.io/campep" style="border: none; width: 900px; height: 1000px"></iframe>


I was somewhat surprised to see that, [despite](http://www.jacmp.org/index.php/jacmp/article/view/4729/html) much [naysaying](http://www.jacmp.org/index.php/jacmp/article/view/4932/html_35) on the part of [JACMP](http://www.jacmp.org/index.php/jacmp/article/view/4932/html_35), MSc degrees have similar rates for obtaining residencies as do their PhD counterparts.

There are several things to note about the above app:

* Not every school actually publishes their stats. I made every effort to find stats from each school, but several eluded me.
* Stats for 2014 are scarce, and some schools haven't yet published 2013 data.
* The degree type label "Both MSc + PhD" implies the published stats were not differentiated
* DMP degrees were labeled as PhD
* Any school not on the list (and there are several) did not have the proper data published




