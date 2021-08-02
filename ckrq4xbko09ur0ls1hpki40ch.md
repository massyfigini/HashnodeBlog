## Regression on Python (part 1)


```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
``` 

Here are the data used:
``` 
path = 'https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DA0101EN/automobileEDA.csv'
df = pd.read_csv(path)
df.head()
``` 

### SIMPLE LINEAR REGRESSION
``` 
from sklearn.linear_model import LinearRegression

#create the linear regression object
lm = LinearRegression()

#highway-mpg for predict price
X = df[['highway-mpg']]
Y = df[['price']]
lm.fit(X,Y)

#predict for first 5 raw of the table
lm.predict(X[0:5])

#value of intercept and coefficient
lm.intercept_
lm.coef_
``` 

### MULTIPLE LINEAR REGRESSION

``` 
Z = df[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']]

lm.fit(Z, df['price'])
``` 


### MODEL EVALUATION USING VISUALIZATION

``` 
import seaborn as sns
``` 
**Simple linear regression**
``` 
width = 12
height = 10
plt.figure(figsize=(width, height))
sns.regplot(x="highway-mpg", y="price", data=df)
plt.ylim(0,)     #y axis starts from 0
``` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627637741008/Ie3szNZxs.png)

Is more correlated with price "peak-rpm" or "highway-mpg"?  
``` 
df[["peak-rpm","highway-mpg","price"]].corr()
``` 
highway-mpg is more correlated:
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627637904513/1tlfgsRPl.png)

Residual Plot it's a graph that shows the residuals on the vertical y-axis and the independent variable on the horizontal x-axis.  
We can see from this residual plot that the residuals are not randomly spread around the x-axis, which leads us to believe that maybe a non-linear model is more appropriate for this data.  
``` 
width = 12
height = 10
plt.figure(figsize=(width, height))
sns.residplot(df['highway-mpg'], df['price'])
plt.show()
``` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627637981764/j6vUahbEc.png)

**Multiple linear regression**
``` 
#distribution plot: fitted values vs actual values
Y_hat = lm.predict(Z)    #make the prediction

plt.figure(figsize=(width, height))
ax1 = sns.distplot(df['price'], hist=False, color="r", label="Actual Value")
sns.distplot(Y_hat, hist=False, color="b", label="Fitted Values" , ax=ax1)
plt.title('Actual vs Fitted Values for Price')
plt.xlabel('Price (in dollars)')
plt.ylabel('Proportion of Cars')
plt.show()
plt.close()
``` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627638972470/-WMh2FF0f.png)

Fitted reasonable close to actual, but there is space for improvement  


### POLYNOMIAL REGRESSION

Function for plotting the data:
``` 
def PlotPolly(model, independent_variable, dependent_variabble, Name):
    x_new = np.linspace(15, 55, 100)
    y_new = model(x_new)
    plt.plot(independent_variable, dependent_variabble, '.', x_new, y_new, '-')
    plt.title('Polynomial Fit with Matplotlib for Price ~ Length')
    ax = plt.gca()
    ax.set_facecolor((0.898, 0.898, 0.898))
    fig = plt.gcf()
    plt.xlabel(Name)
    plt.ylabel('Price of Cars')
    plt.show()
    plt.close()

#variables
x = df['highway-mpg']
y = df['price']
``` 

Here we use a polynomial of the 3rd order (cubic):
``` 
f = np.polyfit(x, y, 3)
p = np.poly1d(f)
print(p)
PlotPolly(p, x, y, 'highway-mpg')
``` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627639126385/2Hd_Iokww.png)

``` 
#coefficients
np.polyfit(x, y, 3)    # y = a + b1X + b2X² + b3X³
``` 

Multivariate Polynomial function: with four X variables we will have 15 coefficient
``` 
from sklearn.preprocessing import PolynomialFeatures
pr=PolynomialFeatures(degree=2)
pr
Z_pr=pr.fit_transform(Z)
Z.shape     #The original data is of 201 samples and 4 features
Z_pr.shape     #after the transformation, there 201 samples and 15 features
``` 

### MEASURES FOR IN-SAMPLE EVALUATION

**Simple linear regression**:
``` 
lm.fit(X, Y)     #highway_mpg_fit
lm.score(X, Y)
``` 
R^2: 0.4965911884339176
``` 
Yhat=lm.predict(X)     #predictions
from sklearn.metrics import mean_squared_error
mean_squared_error(df['price'], Yhat)
``` 
MSE: 31635042.944639888

**Multiple linear regression**:
``` 
lm.fit(Z, df['price'])     #fit the model
lm.score(Z, df['price'])
``` 
R^2: 0.8093562806577457
``` 
Y_predict_multifit = lm.predict(Z)     #predictions
mean_squared_error(df['price'], Y_predict_multifit)
``` 
MSE: 11980366.87072649

Polynomial linear regression:
``` 
from sklearn.metrics import r2_score     #we use a different function
r2_score(y, p(x)) 
``` 
R^2: 0.674194666390652
``` 
mean_squared_error(df['price'], p(x)) 
``` 
MSE: 20474146.426361218

Comparing these three models, we conclude that the MLR model is the best model to be able to predict price from our dataset.
