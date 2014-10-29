---
layout: page
title: "Brachytherapy Reproducible Research"
permalink: /brachytherapyRepro/
---
##Introduction

In this document I will explain the calculations required to obtain TG-43 parameters for a new brachytherapy seed. The document contains  R functions capable of calculating all required quantities of interest in a reproducible fashion.

##Calculated quantities

###Geometry Function

The first required quantity is the **Geometry Function**.  The purpose of the geometry function is to improve the accuracy with which dose rates can be estimated through interpolation. The line source model is recommended in [TG-43](https://www.aapm.org/pubs/reports/rpt_84.pdf), with geometry as in this diagram:

![geom1](/figs/BrachyReproducible/Geom1.png)

The equation for the Geometry function is as follows:

![geom2](/figs/BrachyReproducible/Geom2.png)

To calculate the geometry function we define several function.  The equations for each side of the diagram, and \\(\beta\\), and use the outputs from these functions to calculate the geometry function:

{% highlight r %}
Beta <- function(theta, r, L=0.3){
    side1 <- sqrt((L/2)^2 + r^2 - L*r*cos(pi - theta))
    side2 <- sqrt((L/2)^2 + r^2 - L*r*cos(theta))
    beta <- acos((side1^2 + side2^2 - L^2)/(2*side1*side2))   
    return(beta)
}    
 
GeomFunction <- function(theta, r, L=0.3){
    if(theta == 0){geom <- 1/(r^2 - (L^2)/4)}
    else{geom <- Beta(theta, r, L)/(L*r*sin(theta))}
    return(geom)    
}

#Example calculation:
GeomFunction(pi/18, 10,0.3)
{% endhighlight %}



{% highlight text %}
## [1] 0.01
{% endhighlight %}

##Measured Quantities

All measured quantities are "measured" using Monte Carlo simulation.  In this case, MCNPX was used for calculation.  The rest of the document will assume that the Mone Carlo calculations are complete, and the output files are organized in a directory structure such as this:
![dirStruct](/figs/BrachyReproducible/DirStructure.png)

###Dose Rate Constant

The output file should look like this: [MCNP output file](https://raw.githubusercontent.com/joelcarlson/Reproducible-TG43-Brachytherapy/master/Aug28KAERIFA.txt).

This output file contains the detector element surfaces relevant to calculating 2D anisotropy, hence the many warnings, however they do not alter the output in any way. A function must be written to extract the relevant detector cell tallies described in this diagram, where F6 indicates the track length estimator tally in MCNPX:

![AirKerm](/figs/BrachyReproducible/AirKerma.png)

Dose rate constant is calculated as:

![drc](/figs/BrachyReproducible/DRC.png)


In the TG-43 formalism,\\(r_0\\) is 1 cm and \\(\theta_0=90^{\circ}\\).

Given the output file we define a function to extract the cell tally values and use them to calculate the dose rate constant of the seed:

{% highlight r %}
library(stringr)

doseRC <- function(upper.cell = 803, lower.cell = 802, r=30, file.path= "DoseRateOutfile/KAERIFreeAir.txt"){
    #Read the output file
    sctext <- scan(file.path, character(0), sep = "\n")
    
    #Find and extract tally value, then trim whitespace and extract value 
    ##Upper tally
    upper <- which(is.na(str_extract(sctext,paste0("cell  ",upper.cell)))==FALSE)
    upper <- sctext[upper + 1]
    upper <- as.numeric(unlist(str_split(str_trim(upper), " "))[1])
    
    ##Lower tally
    lower <- which(is.na(str_extract(sctext,paste0("cell  ",lower.cell)))==FALSE)
    lower <- sctext[lower + 1]
    lower <- as.numeric(unlist(str_split(str_trim(lower), " "))[1])
    
    #calculate air kerma
    doseRC <- lower/(upper * (r^2))
    return(doseRC)
}   

doseRC()
{% endhighlight %}



{% highlight text %}
## [1] 0.9174
{% endhighlight %}

###Radial Dose Function

The next quantity of interest to calculate is the **Radial Dose Function**, \\(g_L(r)\\).  The formula is as follows:

![drc](/figs/BrachyReproducible/radial.png)

Therefore we need to access the tally values from our output from the simulation with the detectors on the transverse plane (ie. \\(\theta_0\\)). The output file should look like this: [example output file](https://raw.githubusercontent.com/joelcarlson/Reproducible-TG43-Brachytherapy/master/KAERI_90_out.txt).

A similar method is used as for the extraction of the dose rate constant.  To note, the input of the function called `cell.range` is the range of cells used to define the detector elements (tallies), and `radii` is a vector containing the distance of each detector element from the seed.


{% highlight r %}
##Find F6 tally value at given cell range and radii, outputs a dataframe containing D(r,90)
Dr90 <- function(cell.range = c(223:246), radii=c(0.25,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1,1.25,1.5,1.75,2,2.5,3,3.5,4,4.5,5,6,7,8,9,10), file.path= "AnisoOutFiles/KAERI_90_out.txt"){
    #Read the output file
    sctext <- scan(file.path, character(0), sep = "\n")
    f6vals <- c()
    for(i in cell.range){
      cellval <- which(is.na(str_extract(sctext,paste0("cell  ",i)))==FALSE)
      cellval <- sctext[cellval + 1]
      cellval <- as.numeric(unlist(str_split(str_trim(cellval), " "))[1])
      f6vals <- append(f6vals, cellval)
    }
    
    Dr90 <- data.frame("r"=radii, "Dr90"=f6vals )
    return(Dr90)
}

##Use our GeomFunction and Dr90 to calculate the radial dose function 
RadialDoseFunction <- function(){
  #Define values
  Dr90 <- Dr90()
  D190 <- Dr90[Dr90$r==1.00,"Dr90"]
  GLr90 <- c()
  for(i in Dr90[,"r"]){
      GLr90 <- append(GLr90,GeomFunction(pi/2, i))
  }
  GL190 <- GeomFunction(pi/2, 1)
  
  RDtable <- data.frame(Dr90, GLr90)
  
  RDtable$gLr <- (RDtable$Dr90 / D190) * (GL190 / RDtable$GLr90)
  return(RDtable)
}

RadialDoseFunction()
{% endhighlight %}



{% highlight text %}
##        r      Dr90     GLr90     gLr
## 1   0.25 3.658e-03 14.411187 1.08665
## 2   0.30 2.611e-03 10.303280 1.08473
## 3   0.40 1.506e-03  5.979511 1.07839
## 4   0.50 9.696e-04  3.886091 1.06812
## 5   0.60 6.718e-04  2.721985 1.05652
## 6   0.70 4.900e-04  2.010413 1.04354
## 7   0.80 3.717e-04  1.544566 1.03029
## 8   0.90 2.901e-04  1.223324 1.01540
## 9   1.00 2.319e-04  0.992600 1.00000
## 10  1.25 1.425e-04  0.636954 0.95755
## 11  1.50 9.441e-05  0.442972 0.91241
## 12  1.75 6.586e-05  0.325734 0.86557
## 13  2.00 4.773e-05  0.249533 0.81896
## 14  2.50 2.714e-05  0.159808 0.72719
## 15  3.00 1.661e-05  0.111019 0.64063
## 16  3.50 1.068e-05  0.081583 0.56059
## 17  4.00 7.134e-06  0.062471 0.48886
## 18  4.50 4.894e-06  0.049364 0.42439
## 19  5.00 3.434e-06  0.039988 0.36762
## 20  6.00 1.779e-06  0.027772 0.27418
## 21  7.00 9.681e-07  0.020405 0.20311
## 22  8.00 5.465e-07  0.015623 0.14974
## 23  9.00 3.176e-07  0.012345 0.11016
## 24 10.00 1.892e-07  0.009999 0.08102
{% endhighlight %}

###2D Anisotropy Function

The next and final calculation is of the **2D anisotropy function**.  The formula for the 2D anisotropy function is:

![ani](/figs/BrachyReproducible/aniso.png)

For this function, the data is stored across several different output files in the folder `AnisoOutFiles`.  There exists one output file every \\(10^{\circ}\\) for a total of 10 files (for publication quality it is recommended that every \\(5^{\circ}\\) be sampled.)

Based on our detector cell definitions, the detectors along \\(\theta = 0\\) go from 7 to 30, \\(\theta = 10\\) from 31 to 54 and so on (\\(\theta = 90\\) from 223 to 246, as in the radial dose function calculation).

The first step is to create a function to extract the tally values and build a data frame based on them:

{% highlight r %}
#Build dataframe of tally results by angle. Output files *MUST* be in order from 00 to 90 degrees
#radii are the distances of detector cells from seed,  
buildTallyDF <- function(file.dir = "AnisoOutFiles", first.cell=7, radii=c(0.25,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1,1.25,1.5,1.75,2,2.5,3,3.5,4,4.5,5,6,7,8,9,10)){
  #Add radii column
  TallyDF <- data.frame("r"=radii)
  #For each file in the directory create vector of appropriate cell values, append to DF
  for(file.name in list.files(file.dir)){
    file <- scan(paste0(file.dir,"/",file.name), character(0), sep = "\n")
    f6vals <- c()
    
    for(i in seq(first.cell, first.cell+length(radii)-1)){
      cellval <- which(is.na(str_extract(file,paste0("cell  ",i)))==FALSE)
      cellval <- file[cellval + 1]
      cellval <- as.numeric(unlist(str_split(str_trim(cellval), " "))[1])
      f6vals <- append(f6vals, cellval)
    }
    
    first.cell <- first.cell + length(radii)
    TallyDF <- data.frame(TallyDF, file.name = f6vals)
    
  }
  #rename columns using their respective angles
  colnames(TallyDF)<- c("r", seq(0,90, by=10))
  return(TallyDF)
}
#buildTallyDF()
{% endhighlight %}

Next, we use the newly created dataframe, `buildAnisoDF()`, to calculate the 2D anisotropy function using the above formula:



{% highlight r %}
Aniso <- function(){
  TallyDF <- buildTallyDF()
  #Divide all tally values by the tally value of that radius at 90 degrees
  AnisoDF <- TallyDF[,-1]/TallyDF[,"90"]
  AnisoDF <- cbind("r"=TallyDF[,1], AnisoDF)
  
  #At each position of the Aniso dataframe, multiply the aniso value by the geometry function at that point
  for(angle in colnames(AnisoDF[,-1])){
    
    for(radius in AnisoDF[,"r"]){
      
     AnisoDF[AnisoDF$r==radius, angle] <- AnisoDF[AnisoDF$r==radius, angle] * GeomFunction(pi/2, radius) / GeomFunction(as.numeric(angle)*pi/180, radius)
  }}
      
  
  return(AnisoDF) 
  }
  Aniso()
{% endhighlight %}



{% highlight text %}
##        r    0   10   20   30   40   50   60   70   80 90
## 1   0.25 0.20 0.49 0.94 1.05 1.08 1.07 0.97 0.99 1.00  1
## 2   0.30 0.19 0.46 0.82 1.00 1.05 1.07 1.01 0.99 1.00  1
## 3   0.40 0.21 0.47 0.75 0.94 1.02 1.05 1.07 0.99 1.00  1
## 4   0.50 0.22 0.48 0.73 0.91 1.00 1.04 1.06 1.06 1.00  1
## 5   0.60 0.24 0.51 0.73 0.89 0.99 1.04 1.06 1.07 1.00  1
## 6   0.70 0.26 0.52 0.74 0.89 0.99 1.03 1.06 1.06 1.00  1
## 7   0.80 0.28 0.54 0.74 0.88 0.98 1.03 1.05 1.06 1.00  1
## 8   0.90 0.29 0.55 0.75 0.88 0.98 1.03 1.05 1.06 1.05  1
## 9   1.00 0.31 0.56 0.75 0.88 0.97 1.02 1.05 1.06 1.06  1
## 10  1.25 0.35 0.59 0.76 0.88 0.97 1.02 1.04 1.06 1.06  1
## 11  1.50 0.38 0.61 0.77 0.88 0.96 1.02 1.04 1.05 1.05  1
## 12  1.75 0.41 0.63 0.78 0.89 0.96 1.01 1.04 1.05 1.05  1
## 13  2.00 0.42 0.64 0.79 0.89 0.96 1.01 1.03 1.05 1.05  1
## 14  2.50 0.46 0.66 0.80 0.89 0.96 1.01 1.03 1.04 1.05  1
## 15  3.00 0.49 0.68 0.81 0.90 0.96 1.00 1.03 1.04 1.04  1
## 16  3.50 0.53 0.70 0.81 0.90 0.96 1.00 1.02 1.04 1.04  1
## 17  4.00 0.57 0.71 0.82 0.90 0.96 1.00 1.02 1.03 1.04  1
## 18  4.50 0.59 0.72 0.83 0.90 0.96 1.00 1.02 1.03 1.03  1
## 19  5.00 0.63 0.73 0.83 0.90 0.95 0.99 1.02 1.03 1.03  1
## 20  6.00 0.65 0.74 0.84 0.90 0.95 0.99 1.01 1.02 1.03  1
## 21  7.00 0.68 0.76 0.84 0.90 0.95 0.99 1.01 1.02 1.02  1
## 22  8.00 0.69 0.76 0.85 0.91 0.95 0.99 1.01 1.02 1.02  1
## 23  9.00 0.72 0.77 0.85 0.91 0.95 0.99 1.01 1.02 1.02  1
## 24 10.00 0.74 0.78 0.85 0.91 0.95 0.98 1.00 1.01 1.02  1
{% endhighlight %}

##Plots

###Radial Dose Function

Here is the radial dose function plotted against several other published seed results (other seed data from external .csv):


{% highlight r %}
radialPlot()
{% endhighlight %}

![center](/figs/BrachyReproducible/unnamed-chunk-7.png) 

And a zoom in of the area of interest from r = 0 to r = 2:



{% highlight r %}
radialPlotZoom()
{% endhighlight %}

![center](/figs/BrachyReproducible/unnamed-chunk-9.png) 

###2D Anisotropy

Inspiration for the following plot was taken from [Rivard 2008](http://scitation.aip.org/content/aapm/journal/medphys/36/2/10.1118/1.3056463).  In this plot the dotted lines are the KAERI seed, and the solid lines the IAI-125 seed.



{% highlight r %}
plotAniso()  
{% endhighlight %}

![center](/figs/BrachyReproducible/unnamed-chunk-11.png) 

##Conclusion

Calculations for TG-43 parameters of a novel brachytherapy seed were presented. The functions should be generalizable, perhaps with some slight modifications.

  
