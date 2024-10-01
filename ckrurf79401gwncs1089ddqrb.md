---
title: "Linear Regression with R"
datePublished: Tue Nov 01 2016 16:00:14 GMT+0000 (Coordinated Universal Time)
cuid: ckrurf79401gwncs1089ddqrb
slug: linear-regression-with-r
tags: r

---

Linear Regression:

> Yi = beta0 + beta1Xi + ei

```sql
lm(y~x, tab)   #y dependent variable, x regressor
coef(lm(y~x, tab))  #estract the coefficient beta0 and beta1
summary(lm(y~x, tab))   #detailed summary
```

To print only a parameter:

```sql
summary(lm(y~x, tabella))$sigma   #only residual sd
```

To plot the regression line (g is a pre builded graph):

```sql
g + geom.smooth(method="lm", formula=y~x)   #formula is optional (y~x is the default)
```

I() if I want to use a function:

```sql
lm(y~I(x-mean(x)), data=tab))
```

beta1 (slope) unchanged (y increase for each increase of x)  
beta0 (intercept) became y expected for each average value of x

To predict using the regression line:

```sql
Predict(lm(y~x, tab), newdata=data.frame(x=z))   #z is a vector with x values
```

Residuals:

```sql
resid(lm(y~x, tab))
```

Without "newdata" it uses the lm parameters, so it calculates residuals for each x and y

### SUMMARY LM FUNCTION

model &lt;- lm(y ~ 1)           Intercept only (no slope coefficient)  
model &lt;- lm(y ~ x - 1)     Slope only (no intercept coefficient)  
model &lt;- lm(y ~ x)           Intercept & slope (x is not a factor)  
model &lt;- lm(y ~ w)          Intercepts only (w is a factor)  
model &lt;- lm(y ~ x + w)    Intercepts & slope against confounding x & w  
model &lt;- lm(y ~ x \* w)     Intercepts & slopes against interacting x & w

lm(y ~ 1)           yhat = B0  
lm(y ~ x - 1)     yhat = B1*x  
lm(y ~ x)           yhat = B0 + B1*x  
lm(y ~ w)          yhat = B0 + B2*w  
lm(y ~ x + w)    yhat = B0 + B1*x + B2*w  
lm(y ~ x \* w)       yhat = B0 + B1*x + B2*w + B3*x\*w

lm(y ~ I(log(x)) + w)     yhat = B0 + B1*log(x) + B2*w  
lm(y ~ I(log(x)) \* w)      yhat = B0 + B1*log(x) + B2*w + B3\*log(x)\*w