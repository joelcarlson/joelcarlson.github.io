---
title: 'Monte Carlo Example Post'
author: "Joel Carlson"
layout: page
excerpt: An example of a more complicated markdown document.  Monte Carlo methods for the calculation of function defined shapes.
---
This is just an example to see if a more complicated .rmd file can survive conversion from .rmd to .md


###Question 2 a)
Calculate the area encompassed by \\(x^2 + 4y^2 = 4\\)

The x intercept:  
\\(x^2 + 4y^2 = 4\\) 
\\(x^2 + 4(0)^2 = 4\\)  
\\(x = \sqrt{4} = 2\\)  

And y intercept:  
\\((0)^2 + 4y^2 = 4\\)  
\\(y = \sqrt{4/4} = 1\\)  

So we will choose random numbers from \\(0 \leq x \leq 2\\) and \\(0 \leq y \leq 1\\) and test whether or not fall inside or outside of our function.

Below is a function which has three steps:

* Create a data frame of a given size with random $x$ and $y$ values within our defined intercept bounds

* Create a column of the function evaluated using the $x$ and $y$ values

* Create a new column that is `1` if the $x$ and $y$ values are below our function


{% highlight r %}
valueCalc <- function(iterations){
    #Create data frame
    values <- data.frame(x=runif(iterations, 0, 2), y=runif(iterations, 0, 1))
    #Calculations
    values$calc <- values$x^2 + (4 * values$y^2)
    #Create counting column 
    values$inside <- ifelse(values$calc < 4, 1, 0)
    
    values
} 
{% endhighlight %}
Let's look at a quick example with only 5 iterations:

{% highlight r %}
valueCalc(5)
{% endhighlight %}



{% highlight text %}
##       x       y  calc inside
## 1 1.830 0.01203 3.351      1
## 2 1.527 0.75843 4.632      0
## 3 1.611 0.49895 3.592      1
## 4 1.468 0.46611 3.023      1
## 5 1.364 0.90101 5.108      0
{% endhighlight %}

Lets look at a plot of the given function, and 10 random points, and 100 random points:


![center](/figs/Example Post - MC.rmd/unnamed-chunk-5.png) 

To calculate the area of the defined shape we note that the area of the square is \\(2 * 1 = 2\\), and a certain proportion of that area is taken up by the defined shape (area below the function). By looking at the proportion of the random points that fall inside *vs* outside the shape (that is, the sum of our `inside` column), we can guess its area.


{% highlight r %}
#100 iterations
hundred <- valueCalc(100)
sum(hundred$inside)/100
{% endhighlight %}



{% highlight text %}
## [1] 0.82
{% endhighlight %}



{% highlight r %}
#10000 iterations
tenthousand <- valueCalc(10000)
sum(tenthousand$inside)/10000
{% endhighlight %}



{% highlight text %}
## [1] 0.7807
{% endhighlight %}



{% highlight r %}
#1000000 iterations
million <- valueCalc(1000000)
sum(million$inside)/1000000
{% endhighlight %}



{% highlight text %}
## [1] 0.7849
{% endhighlight %}

This is just \\(\frac{1}{4}\\) of the shape, however.  The shape is symmetrical, so the proportion of points inside *vs* outside of the function will remain constant. In an area \\(4\\) times as large, or \\(4 * 2 = 8 m^2\\), \\(78.49\%\\) of the area is taken up by the shape.  That is \\(0.7849 * 8m^2 = 6.2792m^2\\) is the area. 

We can confirm this with an integral:
\\(2\int_{-2}^{2}\sqrt{\frac{4-x^2}{4}}\,dx = 2pi \approx 6.28\\)


###Question 2 b)
A similar procedure as *Question 2 a)* is followed.  This time we want the relative area of the shape defined by: \\((x-1)^2 + y^2 = 1\\).

This is a circle shifted by \\(1\\) to the right on the \\(x-axis\\).  Thus the intercepts are \\(x = 0, 2\\) and \\(y = 0\\)

The shift moving the center of the cicle won't change our boundaries in this case, we will just have to be more careful at the end when we are talking about relative proportions.


We will define a similar function:

{% highlight r %}
valueCalcb <- function(iterations){
    #Create data frame
    values <- data.frame(x=runif(iterations, 0, 2), y=runif(iterations, 0, 1))
    #Calculations
    values$calc <- (values$x - 1)^2 + values$y^2
    #Create counting column 
    values$inside <- ifelse(values$calc < 1, 1, 0)
    
    values
} 
{% endhighlight %}

{% highlight r %}
valueCalcb(5)
{% endhighlight %}



{% highlight text %}
##        x        y    calc inside
## 1 1.4803 0.504945 0.48562      1
## 2 0.7941 0.001354 0.04239      1
## 3 0.8626 0.776546 0.62190      1
## 4 1.1135 0.325987 0.11916      1
## 5 0.4893 0.990583 1.24206      0
{% endhighlight %}

And plotted:

![center](/figs/Example Post - MC.rmd/unnamed-chunk-10.png) 

With proportions:

{% highlight r %}
#1000000 iterations
millionb <- valueCalcb(1000000)
sum(millionb$inside)/1000000
{% endhighlight %}



{% highlight text %}
## [1] 0.7863
{% endhighlight %}

This time our square area is of size \\(2 * 2 = 4m^2\\) and the proportion of hits inside the shape is \\(78.63\%\\) thus the area is \\(0.7863 * 4m^2 = 3.1452 \approx pi\\).  This makes sense, as it is a circle of \\(radius = 1\\), merely shifted to the side. 
