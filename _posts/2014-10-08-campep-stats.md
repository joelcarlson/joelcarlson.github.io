---
title: "Interactive Campep Statistics"
author: "Joel Carlson"
layout: page
excerpt: A shiny app for analysis of enrolment and employment for Medical Physics students
---

To practice as a Medical Physicist in North America one has to become certified by the [ABR](http://www.theabr.org/).  As of 2014, only those who have graduated from a [CAMPEP](http://www.campep.org/) accredited Medical Physics residency are eligible to take the ABR exams, and thus able to pursue the practice. 

Something nice that has come from this is that CAMPEP requires schools to post several statistics pertaining to enrolment and employment of their students.  However, there are no regulations for *how* this data is to be presented, resulting in a situation where each school posts the stats in a different way.  Some schools post a .pdf on their homepage, others bury it with grad school admission statistics, others are accessible using only the skeleton key provided to the dean.

I wanted to get an overview of these statistics, and to do so I combed through all of the schools on the accredited schools [list](http://www.campep.org/campeplstgrad.asp) and compiled it here, with a convenient [Shiny](http://shiny.rstudio.com/) app to allow some data exploration.  Here is the app:

<iframe src="https://joelcarlson.shinyapps.io/campep" style="border: none; width: 900px; height: 1100px"></iframe>


I was somewhat surprised to see that, [despite](http://www.jacmp.org/index.php/jacmp/article/view/4729/html) much [naysaying](http://www.jacmp.org/index.php/jacmp/article/view/4932/html_35) on the part of [JACMP](http://www.jacmp.org/index.php/jacmp/article/view/4932/html_35), MSc degrees have similar rates for obtaining residencies as do their PhD counterparts.

Of note also is the approximate number of graduates.  JACMP has mentioned that in North America there is a need for 150-175 new Medical Physicists each year, however the number of graduates in 2012 was close to 250. The number of graduates drops under 150 for 2013, but this may be due to incomplete reporting of 2013 statistics at the time of data compilation. Clearly there is a bit of a mismatch between supply and demand. CAMPEP has stated that they are working to increase the number of residency positions to meet the increased demand.

There are several things to note about the above app:

* Not every school actually publishes their stats. I made every effort to find stats from each school, but several eluded me.
* Stats for 2014 are scarce, and some schools haven't yet published 2013 data.
* The degree type label "Both MSc + PhD" implies the published stats were not differentiated
* DMP degrees were labeled as PhD
* Any school not on the list (and there are several) did not have the proper data published




