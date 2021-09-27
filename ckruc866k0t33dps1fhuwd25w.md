## Coursera Data Science Specialization: Statistical Inference Course Project part 1

Part 1 of my  [Statistical Inference](https://www.coursera.org/learn/statistical-inference)  project, part of the  [Johns Hopkins Data Science Specialization on Coursera](https://www.coursera.org/specializations/jhu-data-science).  
You can find the [.Rmd (R Markdown) format here](https://github.com/massyfigini/StatisticalInferenceCP/blob/master/Statistical_Inference_CP1.Rmd), the  [html result here](https://massyfigini.github.io/assets/Statistical_Inference_CP1.html) and the [Rpubs pubblication here](https://rpubs.com/massyfigini/StatisticalInferenceCP1).  

-------------------------------------------------------------------------------------

# Statistical Inference Course Project Part 1: Simulation
Massimiliano Figini  
October 9, 2016  

Investigation of the exponential distribution in R and comparation with the Central Limit Theorem.  
  
Instruction:  
1) 1000 Simulation  
2) 40 exponentials  
3) Lambda = 0.2  
  
&nbsp;


```r
# Basic settings
knitr::opts_chunk$set(echo = TRUE,tidy.opts=list(width.cutoff=60),tidy=TRUE)
set.seed(1983)
# Main variables
Lambda = 0.2
Simulation <- NULL
for (i in 1 : 1000) Simulation = c(Simulation, mean(rexp(40,Lambda)))
```
  
&nbsp;

### QUESTION 1: Show the sample mean and compare it to the theoretical mean of the distribution.

Theoretical mean is 1/Lambda, sample mean is the mean of the simulation values.


```r
TheoreticalMean <- 1/Lambda
SampleMean <- mean(Simulation)
paste("Theoretical mean is", round(TheoreticalMean, 3), "sample mean is", 
    round(SampleMean, 3))
```

```
## [1] "Theoretical mean is 5 sample mean is 5.028"
```

The sample mean is very close to the theoretical mean.  
  
&nbsp;

### QUESTION 2: Show how variable the sample is (via variance) and compare it to the theoretical variance of the distribution.

I can calculate theoretical variance by dividing 1/Lambda for the square root of the number of exponenentials and squaring all.


```r
TheoreticalVariance <- ((1/Lambda)/sqrt(40))^2
SampleVariance <- var(Simulation)
paste("Theoretical variance is", TheoreticalVariance, "sample variance is", 
    round(SampleVariance, 3))
```

```
## [1] "Theoretical variance is 0.625 sample variance is 0.617"
```

The sample variance is very close to the theoretical variance.  
  
&nbsp;
  
### QUESTION 3:  Show that the distribution is approximately normal.

Using an histogram I can easily see if the distribution is approximately normal.


```r
hist(Simulation, probability = TRUE, breaks = 40, col = "red", 
    main = "Distribution", xlab = "Simulation means")
lines(density(Simulation), lwd = 3, col = "black")
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627890740755/kcH7JJ7e4.png)
  
The distribution of averages of random sampled exponentials is like a normal distribution.  