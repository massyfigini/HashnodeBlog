## Regression on Python (part 2)

### IMPORT PACKAGES

```
import pandas as pd
import numpy as np
! pip install ipywidgets
from IPython.display import display
from IPython.html import widgets
from IPython.display import display
from ipywidgets import interact, interactive, fixed, interact_manual
``` 


### IMPORT DATA
``` 
path = 'https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DA0101EN/module_5_auto.csv'
df = pd.read_csv(path)
df=df._get_numeric_data()
``` 

### FUNCTIONS FOR PLOTTING
``` 
def DistributionPlot(RedFunction, BlueFunction, RedName, BlueName, Title):
    width = 12
    height = 10
    plt.figure(figsize=(width, height))
    ax1 = sns.distplot(RedFunction, hist=False, color="r", label=RedName)
    ax2 = sns.distplot(BlueFunction, hist=False, color="b", label=BlueName, ax=ax1)
    plt.title(Title)
    plt.xlabel('Price (in dollars)')
    plt.ylabel('Proportion of Cars')
    plt.show()
    plt.close()

def PollyPlot(xtrain, xtest, y_train, y_test, lr,poly_transform):
    width = 12
    height = 10
    plt.figure(figsize=(width, height))
    #training data
    #testing data
    # lr:  linear regression object
    #poly_transform:  polynomial transformation object
    xmax=max([xtrain.values.max(), xtest.values.max()])
    xmin=min([xtrain.values.min(), xtest.values.min()])
    x=np.arange(xmin, xmax, 0.1)
    plt.plot(xtrain, y_train, 'ro', label='Training Data')
    plt.plot(xtest, y_test, 'go', label='Test Data')
    plt.plot(x, lr.predict(poly_transform.fit_transform(x.reshape(-1, 1))), label='Predicted Function')
    plt.ylim([-10000, 60000])
    plt.ylabel('Price')
    plt.legend()
``` 

### TRAINING AND TESTING
``` 
y_data = df['price']
x_data=df.drop('price',axis=1)
``` 

### SPLIT DATA INTO TRAINING AND TESTING
``` 
from sklearn.model_selection import train_test_split
x_train, x_test, y_train, y_test = train_test_split(x_data, y_data, test_size=0.15, random_state=1)
print("number of test samples :", x_test.shape[0])
print("number of training samples:",x_train.shape[0])
``` 
number of test samples : 31
number of training samples: 170

### LINEAR REGRESSION
``` 
from sklearn.linear_model import LinearRegression
lre=LinearRegression()
lre.fit(x_train[['horsepower']], y_train)
lre.score(x_test[['horsepower']], y_test)     
``` 
R^2 in test: 0.707688374146705
``` 
lre.score(x_train[['horsepower']], y_train)     
``` 
R^2 in train, generally higher, here lower (?): 0.6449517437659684

### CROSS-VALIDATION SCORE
``` 
from sklearn.model_selection import cross_val_score

Rcross = cross_val_score(lre, x_data[['horsepower']], y_data, cv=4)    #4 folds
Rcross    
``` 
R^2 for each fold: array([0.7746232 , 0.51716687, 0.74785353, 0.04839605])
``` 
from sklearn.model_selection import cross_val_predict
yhat = cross_val_predict(lre,x_data[['horsepower']], y_data,cv=4)    #predict the value
yhat[0:5]
``` 

### OVERFITTING, UNDERFITTING AND MODEL SELECTION  

Multiple regression model:
``` 
lr = LinearRegression()
lr.fit(x_train[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']], y_train)

#prediction using training data
yhat_train = lr.predict(x_train[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']])
yhat_train[0:5]

#prediction using test data
yhat_test = lr.predict(x_test[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']])
yhat_test[0:5]

#packages for plotting
import matplotlib.pyplot as plt
import seaborn as sns
``` 

Distribution for training and test prediction vs actual values:
``` 
Title = 'Distribution  Plot of  Predicted Value Using Training Data vs Training Data Distribution'
DistributionPlot(y_train, yhat_train, "Actual Values (Train)", "Predicted Values (Train)", Title)
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627640347385/k3tWbr4ea.png)
``` 
Title='Distribution  Plot of  Predicted Value Using Test Data vs Data Distribution of Test Data'
DistributionPlot(y_test,yhat_test,"Actual Values (Test)","Predicted Values (Test)",Title)
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627640564088/XExzYNX9s.png)

Overfitting with polynomial regression:
``` 
from sklearn.preprocessing import PolynomialFeatures
x_train, x_test, y_train, y_test = train_test_split(x_data, y_data, test_size=0.45, random_state=0)
pr = PolynomialFeatures(degree=5)
x_train_pr = pr.fit_transform(x_train[['horsepower']])
x_test_pr = pr.fit_transform(x_test[['horsepower']])
pr
poly = LinearRegression()
poly.fit(x_train_pr, y_train)
yhat = poly.predict(x_test_pr)
yhat[0:5]
print("Predicted values:", yhat[0:4])
``` 
Predicted values: [ 6728.65561887  7307.98782321 12213.78770965 18893.24804015]
``` 
print("True values:", y_test[0:4].values)
``` 
True values: [ 6295. 10698. 13860. 13499.]
``` 
PollyPlot(x_train[['horsepower']], x_test[['horsepower']], y_train, y_test, poly,pr)
``` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627640873151/dn6-LKG1o.png)
``` 
poly.score(x_train_pr, y_train)   
``` 
R^2 of training: 0.556771690212023
``` 
poly.score(x_test_pr, y_test)   
```
R^2 of testing:  0.298713403020441

A very low R^2 on testing is a sign of overfitting
``` 
Rsqu_test = []
order = [1, 2, 3, 4]
for n in order:
        pr = PolynomialFeatures(degree=n)
        x_train_pr = pr.fit_transform(x_train[['horsepower']])
        x_test_pr = pr.fit_transform(x_test[['horsepower']])   
        lr.fit(x_train_pr, y_train)
        Rsqu_test.append(lr.score(x_test_pr, y_test))
plt.plot(order, Rsqu_test)
plt.xlabel('order')
plt.ylabel('R^2')
plt.title('R^2 Using Test Data')
plt.text(3, 0.75, 'Maximum R^2 ')   
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627641549207/1CkEecuVT.png)
We see the R^2 gradually increases until an order three polynomial is used. Then the R^2 dramatically decreases at four. We choose the order before the decrease.

**Interactive graph for polynomial regression**
```
def f(order, test_data):
    x_train, x_test, y_train, y_test = train_test_split(x_data, y_data, test_size=test_data, random_state=0)
    pr = PolynomialFeatures(degree=order)
    x_train_pr = pr.fit_transform(x_train[['horsepower']])
    x_test_pr = pr.fit_transform(x_test[['horsepower']])
    poly = LinearRegression()
    poly.fit(x_train_pr,y_train)
    PollyPlot(x_train[['horsepower']], x_test[['horsepower']], y_train,y_test, poly, pr)
interact(f, order=(0, 6, 1), test_data=(0.05, 0.95, 0.05))
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627641645116/aWqM31b5h.png)

### RIDGE REGRESSION
``` 
pr=PolynomialFeatures(degree=2)
x_train_pr=pr.fit_transform(x_train[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg','normalized-losses','symboling']])
x_test_pr=pr.fit_transform(x_test[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg','normalized-losses','symboling']])
from sklearn.linear_model import Ridge
RigeModel=Ridge(alpha=0.1)    #Ridge model
RigeModel.fit(x_train_pr, y_train)     #fit the model
yhat = RigeModel.predict(x_test_pr)     #predict

print('predicted:', yhat[0:4])
print('test set :', y_test[0:4].values)

#We select the value of Alfa that minimizes the test error
Rsqu_test = []
Rsqu_train = []
dummy1 = []
ALFA = 10 * np.array(range(0,1000))
for alfa in ALFA:
    RigeModel = Ridge(alpha=alfa)
    RigeModel.fit(x_train_pr, y_train)
    Rsqu_test.append(RigeModel.score(x_test_pr, y_test))
    Rsqu_train.append(RigeModel.score(x_train_pr, y_train))

#We can plot out the value of R^2 for different Alphas
width = 12
height = 10
plt.figure(figsize=(width, height))
plt.plot(ALFA,Rsqu_test, label='validation data  ')
plt.plot(ALFA,Rsqu_train, 'r', label='training Data ')
plt.xlabel('alpha')
plt.ylabel('R^2')
plt.legend()
``` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627642057187/_VkQ4VKlF.png)
The red line represents the R^2 of the training data, the blue line represents the R^2 on the test data.   


### GRID SEARCH
``` 
from sklearn.model_selection import GridSearchCV
parameters1= [{'alpha': [0.001,0.1,1, 10, 100, 1000, 10000, 100000, 100000]}]
RR=Ridge()    #Ridge region object
Grid1 = GridSearchCV(RR, parameters1,cv=4)    #grid search object
Grid1.fit(x_data[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']], y_data)    #fit the model
``` 
The object finds the best parameter values on the validation data. We can obtain the estimator with the best parameters and assign it to the variable BestRR as follows
``` 
BestRR=Grid1.best_estimator_
#test our model
BestRR.score(x_test[['horsepower', 'curb-weight', 'engine-size', 'highway-mpg']], y_test)
``` 
Best RR Score: 0.8411649831036152
