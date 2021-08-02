## Linear Regression with R

Linear Regression:

> Yi = beta0 + beta1Xi + ei

```
lm(y~x, tab)   #y dependent variable, x regressor
coef(lm(y~x, tab))  #estract the coefficient beta0 and beta1
summary(lm(y~x, tab))   #detailed summary
``` 

To print only a parameter:
``` 
summary(lm(y~x, tabella))$sigma   #only residual sd
``` 

To plot the regression line (g is a pre builded graph):
``` 
g + geom.smooth(method="lm", formula=y~x)   #formula is optional (y~x is the default)
``` 

I() if I want to use a function:
``` 
lm(y~I(x-mean(x)), data=tab))
``` 
beta1 (slope) unchanged (y increase for each increase of x)  
beta0 (intercept) became y expected for each average value of x  

To predict using the regression line:
``` 
Predict(lm(y~x, tab), newdata=data.frame(x=z))   #z is a vector with x values
``` 

Residuals:
``` 
resid(lm(y~x, tab))   
``` 
Without "newdata" it uses the lm parameters, so it calculates residuals for each x and y


### SUMMARY LM FUNCTION

model <- lm(y ~ 1) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Intercept only (no slope coefficient)  
model <- lm(y ~ x - 1) &nbsp;&nbsp;&nbsp; Slope only (no intercept coefficient)  
model <- lm(y ~ x) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Intercept & slope (x is not a factor)  
model <- lm(y ~ w) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Intercepts only (w is a factor)  
model <- lm(y ~ x + w) &nbsp;&nbsp; Intercepts & slope against confounding x & w  
model <- lm(y ~ x * w) &nbsp;&nbsp;&nbsp; Intercepts & slopes against interacting x & w  

lm(y ~ 1) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; yhat = B0  
lm(y ~ x - 1) &nbsp;&nbsp;&nbsp; yhat = B1*x  
lm(y ~ x) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; yhat = B0 + B1*x  
lm(y ~ w) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; yhat = B0 + B2*w  
lm(y ~ x + w) &nbsp;&nbsp; yhat = B0 + B1*x + B2*w  
lm(y ~ x * w) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; yhat = B0 + B1*x + B2*w + B3*x*w  


lm(y ~ I(log(x)) + w) &nbsp;&nbsp;&nbsp; yhat = B0 + B1*log(x) + B2*w  
lm(y ~ I(log(x)) \* w) &nbsp;&nbsp;&nbsp;&nbsp; yhat = B0 + B1*log(x) + B2*w + B3*log(x)*w  