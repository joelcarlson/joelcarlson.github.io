---
title: "The Dangers(?) of Improperly Centering and Scaling Your Data"
author: "Joel Carlson"
date: "August 18, 2015"
layout: page
excerpt: What happens if you center and scale your training and test set data together? Read on to find out!
---

Recently I was at a conference where many of the presentations involved some sort of machine learning. One thing that I noticed was that often times the speakers would make no mention of being careful to not contaminate their test set with information from their training set.
 	
For example, for certain machine learning algorithms, such as support vector machines, centering and scaling your data is essential for the algorithm to perform. Centering is the process of subtracting the mean of the data from each observation to make the mean of the data zero. Scaling brings the range of the data into [-1,1]. If you center and scale your data before splitting it into training and test sets, then you have used information from your training set to make calculations on your test set. This is a problem. Or could be, at least.

This was concerning me at the conference, but I didn't speak up about it because although I had been instructed to avoid this mistake, and it made intuitive sense to me, I had never actually tested to see if there was a significant difference between the two methods. 

In this post, I am going to explore this by assessing the accuracy of support vector machine models on three datasets: 

  - Not centered or scaled
  - Training and testing centered and scaled together
  - Training and testing centered separately
  
For each situation we will train 1000 svm models using the default parameters from the `e1071` R package. Datawise, we will use a set from the [UCI machine learning repository](http://archive.ics.uci.edu/ml/datasets/Wine). The dataset is a series of wine measurements, quantifying things like "Alcohol Content", "Color Intensity", and "Hue" for 178 wines. The goal is to predict the region the wine came from based on the measurements. 

There are actually 13 variables in the data, but we are going to use only the 5 with the highest standard deviation, since this is for demonstration purposes, not pure predictive accuracy.

##Summarizing the data


 
Let's take a quick look at the mean and standard deviation of each column in the data (sorted by SD):


<table>
 <thead>
  <tr>
   <th style="text-align:left;"> Variable </th>
   <th style="text-align:right;"> Mean </th>
   <th style="text-align:right;"> SD </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Proline </td>
   <td style="text-align:right;"> 746.89 </td>
   <td style="text-align:right;"> 314.91 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Magnesium </td>
   <td style="text-align:right;"> 99.74 </td>
   <td style="text-align:right;"> 14.28 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Alcalinity </td>
   <td style="text-align:right;"> 19.49 </td>
   <td style="text-align:right;"> 3.34 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ColorIntensity </td>
   <td style="text-align:right;"> 5.06 </td>
   <td style="text-align:right;"> 2.32 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Acid </td>
   <td style="text-align:right;"> 2.34 </td>
   <td style="text-align:right;"> 1.12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Flavanoids </td>
   <td style="text-align:right;"> 2.03 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Alcohol </td>
   <td style="text-align:right;"> 13.00 </td>
   <td style="text-align:right;"> 0.81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> OpticalDensity </td>
   <td style="text-align:right;"> 2.61 </td>
   <td style="text-align:right;"> 0.71 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Phenols </td>
   <td style="text-align:right;"> 2.30 </td>
   <td style="text-align:right;"> 0.63 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Proanthocyanins </td>
   <td style="text-align:right;"> 1.59 </td>
   <td style="text-align:right;"> 0.57 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ash </td>
   <td style="text-align:right;"> 2.37 </td>
   <td style="text-align:right;"> 0.27 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hue </td>
   <td style="text-align:right;"> 0.96 </td>
   <td style="text-align:right;"> 0.23 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NonFlavanoidPhenols </td>
   <td style="text-align:right;"> 0.36 </td>
   <td style="text-align:right;"> 0.12 </td>
  </tr>
</tbody>
</table>

As previously stated, we are only going to use the five variables with the highest standard deviation (and alcohol, because hey, we're talking about wine!).



##A bit of intuition

Just so that we have a feel for the data, let's plot a variable or two:
We might be able to guess that there is some relationship between alcohol content and region:

![center](/figs/CentreScale/unnamed-chunk-4-1.png) 

And indeed there is, not a big surprise there.

Wikipedia tells me that, during brewing, [Proline](https://en.wikipedia.org/wiki/Proline) may produce haze, so perhaps it would be related to Color Intensity?

![center](/figs/CentreScale/unnamed-chunk-5-1.png) 

As expected. It looks like these variables are going to be able to feed a pretty accurate model. Based on the way the second plot is organized into clusters I imagine even a simple approach like KNN would do well.

##Building the models

The features of this data vary wildly in scale, for example, the range of the `Proline` column is from 278 to 1680, whereas the `Acid` column goes from 0.74 to 5.80. This is an issue for support vector machines (and some other algorithms), so we need to center and scale the data.

For each situation described above, I trained 1000 svm models (with default parameters) and recorded the accuracy on a held out test set. Each model was trained on a random subset of 50% of the data, and tested on the remaining 50%. The test set acccuracy distributions in each situation are presented as histograms.

###No centering and scaling

To demonstrate the importance of centering and scaling for support vector machines, the models were trained on the raw data.

![center](/figs/CentreScale/unnamed-chunk-6-1.png) 

As we can see, the accuracy is pretty abysmal. There are three categories, so the mean of 0.415 is better than guessing, but not by much. Of course, this was expected, centering and scaling are a necessary step for svm's.

###Scaling Training and Test together

This time we scale the training and test together, and then split them apart for training of the model. This means that there is some information from the test set leaking into the training set, and vice versa.

![center](/figs/CentreScale/unnamed-chunk-7-1.png) 

Clearly that made the difference, accuracy is nearly perfect. 

###Scaling separately (the right way)

Well, this is the right way.

![center](/figs/CentreScale/unnamed-chunk-8-1.png) 

So, it appears that there is just the teeensiest increase in accuracy if we do it the right way. Is it statistically significant, though?

###Statistical Significance

We can assess whether or not there is a difference in the mean accuracy between scaling together and scaling separately by using a t-test. From the histograms we can see that the accuracy scores are very roughly normal, so t-tests are appropriate here.


{% highlight text %}
 
 	Welch Two Sample t-test
 
 data: Separate and Together
 t = 3.8542, df = 1996.389, p-value = 0.0001198
 alternative hypothesis: true difference in means is not equal to 0
 95 percent confidence interval:
      0.00179 0.00552
 sample estimates:
 mean of x mean of y 
      0.939 0.936
{% endhighlight %}

![center](/figs/CentreScale/unnamed-chunk-10-1.png) 

The difference is extremely small, but exists, and is statistically significant.  

##Conclusion

As we saw, centering and scaling the data the proper actually, for this dataset, leads to a tiny, but statistically significant **increase** in the mean accuracy of the svm models. I'm surprised, I thought it was going to be the other way around! It would be interesting to see how the size of the dataset, and number of variables in the model impact this difference.

Of course, this was no fully rigorous test, but I think this adds to the evidence that you should always center and scale your training and test data separately (not least because it's the correct way!).

