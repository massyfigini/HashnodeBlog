---
title: "Basic Python Numpy"
datePublished: Sun Aug 28 2016 14:02:22 GMT+0000 (Coordinated Universal Time)
cuid: ckrupd78600uzncs1aw663jik
slug: basic-python-numpy
tags: python, beginner

---

```python
import numpy as np

mylist = [1, 2, 3]   #list
x = np.array(mylist)   #convert list to array Numpy
y = np.array([4, 5, 6])   #built an array Numpy
r = np.arange(36)   #array with number from 0 to 35
r[0]   #first array element
r[0]=10   #change first array element
r[-1]   #last array element
r[1:5]   #array elements from second to fifth
c[3:5] = 300, 400   #change fourth and fifth values
r[-4:]   #last fourth elements
r[-5::-2]   #from the fifth last to the last by 2
m = numpy.array([1,2,3,4],[5,6,7,8])   #new matrix
r.resize((6, 6))   #r to a 6 by 6 matrix
r[2,3]   #2,3 element of the matrix (third row fourth column) 
r[2,3:6]   #third row, column 4 5 and 6
r[:,1:3]   #second and third element, all rows
r[1,:]   #second row, all columns
r[:2, :-1]   #all rows until the third not included, all columns except the last
r[r > 20]   #array with only >20 values
r[r > 20] = 30   #r with 30 instead > 20 values
n = np.arange(30)**2   #array with values from 0 to 29 squared
n = np.arange(0, 30, 2)   #from 0 build an array with number less 30 by 2 (0,2,4,...,28)
n = n.reshape(3, 5)   #n vector to a 3x5 array
o = np.linspace(0, 4, 9)   #9 values array equally from 0 to 4 spaced (0, 0.5, 1, ..., 4)
o.resize(3, 3)   #like reshape but change the original array
np.ones((3, 2))   #new array 3x2 with all 1
np.ones((2, 3), int)   #specify that all int numbers
np.zeros((2, 3))   #2x3 all zeros
np.eye(3)   #3x3 with 1 in the diagonal, 0 elsewhere array
np.diag(y)   #y vector on the diagonal and 0 elsewhere
np.array([1, 2, 3] * 3)   #result: [1,2,3,1,2,3,1,2,3]
np.repeat([1, 2, 3], 3)   #result: [1,1,1,2,2,2,3,3,3]
np.vstack([p, 2*p])   #vertically union of the the two arrays (p e 2*p)
np.hstack([p, 2*p])   #orizzontally union of the the two arrays (p e 2*p)
#I can use +, -, *, /, ** (exponential) directly on numpy arrays
x + y   #[1, 2, 3] + [4, 5, 6] = [5, 7, 9]
x + 2   #[3, 4, 5] 
x * 2   #[2, 4, 6]
x - y   #[-3, -3 , -3]
x * y   #[4, 10, 18]
x / y   #[0.25, 0.4, 0.5]
x ** 2   #[1, 4, 9]
x.dot(y)   #x times y, so 1*4+2*5+3*6=32
len(z)   #row numbers
z.T   #invert rows and columns
z.dtype   #type of z elements
z = z.astype("f")   #convert z elements to float
x.sum(), np.sum(x)   #elements sum
x.max()
x.min()
x.mean()
np.mean(r[:,0])   #average of matrix first column
x.median()
x.std()   #standard deviation
x.argmax()   #array position of max element
x.argmin()
np.corrcoef(r[:,0], r[:,1])  #correlation coefficient between first and second column
np.random.normal(10, 0.5, 1000)   #1000 random array with normal distribution, mean = 10, std = 0.5
z = np.array([[7, 8, 9], [10, 11, 12]])   #multi dimentional array
z.shape   #rows and column of md array z
z[1][2]   #second row, third column element
z[0][0,1]   #first row, first and second column of md array
z.T   #traspose matrix
```

For loop on numpy arrays:

```python
test = np.random.randint(0, 10, (4,3))   #matrix from 0 to 9

#row cycle:
for row in test:
    print(row)

#index cycle:
for i in range(len(test)):
    print(test[i])

#row and index cycle:
for i, row in enumerate(test):
    print('row', i, 'is', row)

#with zip function can easily iterate on more than one arrays
for i, j in zip(test, test2):
    print(i,'+',j,'=',i+j)
```