## R ggplot2 intro

Package "ggplot2"  


```
library(ggplot2)  
``` 

2 main functions: qplot() and ggplot(), more flexible  

### qplot()  
It plots in a single function  
```
#scatter plot with axis label
qplot(x, y, data=tab)

#color label points based on z
qplot(x, y, data=tab, color=z)

#now with trend line 95% confidence interval, point refers to data, smooth to line
qplot(x, y, data=tab, color=z, geom=c("point","smooth"))

#facets for subplots, before ~ is rows number, after ~ columns number
qplot(x, y, data=tab, facets=.~z)
qplot(x, y, data=tab, geom=c("point","smooth"), facets=.~z)

#boxplot instead scatter plot
qplot(x, y, data=tab, geom="boxplot")

#separate boxplot and labeled with z
qplot(x, y, data=tab, geom="boxplot", color=z)

#histogram because only 1 variable, fill based on w
qplot(z, data=tab, fill=w)

#binwidth gives the bins width
qplot(z ,data=tab, binwidth=2)

#bar plot of z based on y values
qplot(z ,data=tab, geom="bar", weight=y)
``` 

### ggplot()  
Graph built in more steps  

``` 
#x and y from tab data fram
g <- ggplot(tab, aes(x, y))

summary(g)

#empty plot
g

#scatter plot
g+geom_point()

#scatter plot with trend line
g+geom_point()+geom_smooth()

#trend line built with linear regression, se FALSE to not see the confidence interval
g+geom_point()+geom_smooth(method="lm", se=FALSE)   

#divided in w parts
g+geom_point()+geom_smooth(method="lm")+facet_grid(.~w)

#red point with size 4 and transparency to see density
g+geom_point(color="red", size=4, alpha=1/2)

#different colors based on z
g+geom_point(size=4, alpha=1/2, aes(color=z))

#xlab(), ylab(), and ggtitle() for title axis and plot or we can use labs()
g+geom_point(aes(color=drv))+labs(title="Title!", x="X",y="Y")

# theme_bw() put out the grey background, base_family for font
g+geom_point(aes(color=drv))+theme_bw(base_family="Times")   

#line graph with defined range, outliers outside the range are excluded
g + geom_line()+ylim(-3,3)

#margins adds subplot with totals
g+geom_point()+facet_grid(w~z, margins=TRUE)

#w boxplots
g+geom_boxplot()+facet_grid(.~w)   

# barplot: stat='identity' if a column contains y values (no sum required)
g+geom_bar(stat='identity')   

#for plot x label use scale_x_continuous for continuous variable, scale_x_discrete for discrete
g <- ggplot(tab, aes(x, y))+scale_x_continuous(breaks = c(1,2,3,4))
``` 