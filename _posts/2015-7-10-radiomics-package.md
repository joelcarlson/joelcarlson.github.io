---
title: "Introducing radiomics for R"
author: "Joel Carlson"
layout: page
excerpt: Texture analysis matrices and features
---

The `radiomics` package is a set of tools for computing texture matrices from images, and features derived from the matrices. 

You can install the package using:


{% highlight r %}
devtools::install_github("joelcarlson/radiomics")
{% endhighlight %}

#Texture Matrices

In the package are functions for calculating several different types of matrices used to quantify the texture of an image in the form of a matrix.

##Gray Level Co-occurrence Matrix

The first texture matrix is the gray level co-occurence matrix (GLCM). GLCMs take an image (as a matrix), an angle ("0", "45", "90", or "135"), and an integer distance, d. The axes of the GLCM are defined by the grey levels present in the image. Each pixel of the image is scanned and stored as a "reference pixel". The reference pixel is then compared to the pixel that is distance d at angle theta (where "0" degrees is the pixel to the right, "90" is the pixel above) away from the reference pixel, known as the neighbor pixel. Each time a reference value and neighbor value pair is found, the corresponding row and column of the GLCM is incremented by 1. 

A visual example shows this process. Pixels in the image are colored and labelled by grey value. The GLCM is set up such that each pixel value is represented on each axis.

<img src="http://i.imgur.com/m9MKq1I.png" height="227px" width="500px" />

We count the number of times each pair of grey levels occurs, for example, if angle = "0" and d = 1, for the grey level of 1, there is one 1:1 pair:

<img src="http://i.imgur.com/z5xFw6C.png" height="227px" width="500px" />

While there are two 1:3 pairs:

<img src="http://i.imgur.com/4L8dSgf.png" height="227px" width="500px" />

Continuing in this fashion for each grey level, we end up with a GLCM filled with counts.

By convention, we sum the GLCM and the GLCMs transpose to obtain a symmetric matrix:

<img src="http://i.imgur.com/GdVzVxN.png" height="227px" width="829px" />

This can be accomplished using the `radiomics` package as follows:


{% highlight r %}
library(radiomics)
image <- matrix(c(1,2,3,4,1,1,2,4,3,3,3,2,4,1,4,1), nrow=4)
glcm(image, normalize=FALSE)
{% endhighlight %}



{% highlight text %}
##   1 2 3 4
## 1 2 2 3 0
## 2 2 0 2 1
## 3 3 2 0 2
## 4 0 1 2 2
{% endhighlight %}

Normally, GLCMs are normalized before features are calculated, this is the default situation for the `glcm()` function.

More information about the GLCM can be found [here.](http://www.fp.ucalgary.ca/mhallbey/tutorial.htm)

##Gray Level Run Length Matrix

The GLRLM is a matrix which attempts to quantify runs of the same grey level in the image. The GLRLM is set up slightly differently than the GLCM; instead of having grey levels along the abscissa of the table the GLRLM has run lengths.

As with the GLCM, an angle is required (one of "0", "45", "90", or "135"). Below is an example using "0", note that the image matrix is not the same as the GLCM example:

<img src="http://i.imgur.com/aTapTC1.png" height="227px" width="478x" />

For each run of a given length we count how many times that length occurs for each grey level. Here, a run length of 1 pixel occurs 3 times for the yellow pixels, and a run length of 3 occurs once for the blue pixels.

<img src="http://i.imgur.com/u87fW55.png" height="227px" width="545px" />

We complete this for all pixels in the image to obtain the final GLRLM:

<img src="http://i.imgur.com/b61vqV2.png" height="227px" width="545px" />

This can be accomplished using the radiomics package with the command:


{% highlight r %}
image <- matrix(c(1,2,4,2,1,3,4,4,2,2,3,4,2,3,3,4), nrow=4)
glrlm(image)
{% endhighlight %}



{% highlight text %}
##   1 2 3 4
## 1 0 1 0 0
## 2 3 1 0 0
## 3 2 1 0 0
## 4 0 1 1 0
{% endhighlight %}

Note the extra run length column, the number of run length columns is a function of the dimensions of the image. If the run length is possible (in this case, if there were 4 pixels in a row), there will be a column for it in the output. This output can be truncated by specifying the maximum run length you wish to search for:


{% highlight r %}
glrlm(image, max_run_length=3)
{% endhighlight %}



{% highlight text %}
##   1 2 3
## 1 0 1 0
## 2 3 1 0
## 3 2 1 0
## 4 0 1 1
{% endhighlight %}


##Gray Level Size Zone Matrix

The goal of the GLSZM is to quantify regions of contiguous pixels in the image. The GLSZM is set up in the same way as the GLRLM, except along the abscissa are size zones rather than run lengths. A size zone is defined as a collection of 9-connected pixels (ie. connected on their edges and corners) of the same grey level.

For the GLSZM we will use the same image as for the GLCM. Rather than labeling grey levels, contiguous size zones are labeled with their size:

<img src="http://i.imgur.com/vNQTAsz.png" height="227px" width="496px" />

For each color we count the number of zones of a given size, here for example, among other size zones, there are two red zones of size 1, a single blue zone of size 2, and a single light blue zone of size 3:

<img src="http://i.imgur.com/UezvMmX.png" height="227px" width="496px" />

GLSZMs can be calculated using the `glszm` function:


{% highlight r %}
image <- matrix(c(1,2,3,4,1,1,2,4,3,3,3,2,4,1,4,1), nrow=4)
glszm(image)
{% endhighlight %}



{% highlight text %}
##   1 2 3
## 1 2 0 1
## 2 0 0 1
## 3 1 0 1
## 4 2 1 0
{% endhighlight %}

For more information on the GLSZM see [here.](http://thibault.biz/Research/ThibaultMatrices/GLSZM/GLSZM.html)


#Calculating Features

From the texture matrices it is possible to calculate many different features. These are summarized in the following table:


<table>
<thead>
<tr class="header">
<th align="left">First Order</th>
<th align="left">GLCM</th>
<th align="left">GLRLM</th>
<th align="left">GLSZM</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">energy</td>
<td align="left">mean</td>
<td align="left">GLN</td>
<td align="left">SAE</td>
</tr>
<tr class="even">
<td align="left">entropy</td>
<td align="left">variance</td>
<td align="left">HGLRE</td>
<td align="left">LAE</td>
</tr>
<tr class="odd">
<td align="left">kurtosis</td>
<td align="left">autoCorrelation</td>
<td align="left">LRE</td>
<td align="left">IV</td>
</tr>
<tr class="even">
<td align="left">meanDeviation</td>
<td align="left">cProminence</td>
<td align="left">LRHGLE</td>
<td align="left">SZV</td>
</tr>
<tr class="odd">
<td align="left">skewness</td>
<td align="left">cShade</td>
<td align="left">LRLGLE</td>
<td align="left">ZP</td>
</tr>
<tr class="even">
<td align="left">uniformity</td>
<td align="left">cTendency</td>
<td align="left">LGLRE</td>
<td align="left">LIE</td>
</tr>
<tr class="odd">
<td align="left">mean</td>
<td align="left">contrast</td>
<td align="left">RLN</td>
<td align="left">HIE</td>
</tr>
<tr class="even">
<td align="left">median</td>
<td align="left">correlation</td>
<td align="left">RP</td>
<td align="left">LISAE</td>
</tr>
<tr class="odd">
<td align="left">max</td>
<td align="left">differenceEntropy</td>
<td align="left">SRE</td>
<td align="left">HISAE</td>
</tr>
<tr class="even">
<td align="left">min</td>
<td align="left">dissimilarity</td>
<td align="left">SRHGLE</td>
<td align="left">LILAE</td>
</tr>
<tr class="odd">
<td align="left">diff</td>
<td align="left">energy</td>
<td align="left">SRLGLE</td>
<td align="left">HILAE</td>
</tr>
<tr class="even">
<td align="left">var</td>
<td align="left">entropy</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
<tr class="odd">
<td align="left">RMS</td>
<td align="left">homogeneity1</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
<tr class="even">
<td align="left">sd</td>
<td align="left">homogeneity2</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
<tr class="odd">
<td align="left">-</td>
<td align="left">IDMN</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
<tr class="even">
<td align="left">-</td>
<td align="left">IDN</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
<tr class="odd">
<td align="left">-</td>
<td align="left">inverseVariance</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
<tr class="even">
<td align="left">-</td>
<td align="left">maxProb</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
<tr class="odd">
<td align="left">-</td>
<td align="left">sumAverage</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
<tr class="even">
<td align="left">-</td>
<td align="left">sumEntropy</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
<tr class="odd">
<td align="left">-</td>
<td align="left">sumVariance</td>
<td align="left">-</td>
<td align="left">-</td>
</tr>
</tbody>
</table>

Further information and mathematical definitions of these quantities can be found [here.](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0102107#s5)

In the `radiomics` package, each feature associated with a given matrix can be calculated using the `calc_features()` function. For example:

First order features are calculated on the image, and are prefixed with 'calc':
{% highlight r %}
calc_features(hallbey)
{% endhighlight %}

GLCM features are calculated if `calc_features()` is passed a GLCM:
{% highlight r %}
hbGLCM <- glcm(hallbey)
calc_features(hbGLCM)
{% endhighlight %}

And so on for the other texture matrix types.

