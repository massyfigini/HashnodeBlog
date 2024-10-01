---
title: "Simple Linear Regression on Python with scikit-learn"
datePublished: Sat Aug 10 2019 08:25:14 GMT+0000 (Coordinated Universal Time)
cuid: ckrq32zo409f20ls1acnwhcvu
slug: simple-linear-regression-on-python-with-scikit-learn
tags: data, python, data-science, machine-learning

---

```sql
import matplotlib.pyplot as plt
import numpy as np
```

my dataframe is df

```sql
#plot for see if there is a linear relation
plt.scatter(col1, col2)
plt.show()

#create training and test data set
rand = np.random.rand(len(df)) < 0.8
train = cdf[rand]
test = cdf[~rand]
```

### MODEL

```sql
from sklearn import linear_model
regr = linear_model.LinearRegression()
train_x = np.asanyarray(train[['col1']])
train_y = np.asanyarray(train[['col2']])
regr.fit (train_x, train_y)

#coefficients
print ('Coefficients: ', regr.coef_)
print ('Intercept: ',regr.intercept_)

#plot output
plt.scatter(train.col1, train.col2)
plt.plot(train_x, regr.coef_[0][0]*train_x + regr.intercept_[0], color='red')
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627633501559/1bONocaj9.png align="left")

### EVALUATION

```sql
from sklearn.metrics import r2_score

test_x = np.asanyarray(test[['col1']])
test_y = np.asanyarray(test[['col2']])
test_y_ = regr.predict(test_x)

print("Mean absolute error: %.2f" % np.mean(np.absolute(test_y_ - test_y)))
print("Residual sum of squares (MSE): %.2f" % np.mean((test_y_ - test_y) ** 2))
print("R2-score: %.2f" % r2_score(test_y_ , test_y) )
```