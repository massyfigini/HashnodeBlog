---
title: "Basic Python"
datePublished: Sat Apr 30 2016 14:59:41 GMT+0000 (Coordinated Universal Time)
cuid: ckrqh3cim0e5azcs1c6mc3ope
slug: basic-python
tags: python, beginner

---

### Basics

* \# for comments
    
* string with " or '
    
* ; to have multiple commands in the same line
    
* case sensitive
    

### Operators

* +, -, \*, /
    
* \*\* exponential
    
* % division rest
    
* // division without rest (int)
    
* \+ also to concatenate strings
    
* \* also with strings: "a"\*5 = "aaaaa"
    
* &lt;, &gt;. &lt;=. &gt;=, == (equal), != (different) also with strings
    
* logical operators: and, or, not
    

### Variables

\= to assign a variable

```python
altezza = 1.80
a = "ciao"
```

### Types

* int, float, str, bool, function
    
* Type(variable) to see the type
    

### Strings

```python
Name = 'Cecilia'
Name[0]   #'C'
Name[1]   #'e'
Name[-1]   #'a'
Name[0:4]   #from 0 to 4 excluded: 'Ceci'
```

### Tuples

Immutable

```python
tuple1 = (0,10,2,4)
tuple2 = ('a',1,1.5)
tuple2[0]: 'a'
tuple2[0:2]: ('a',1)
tuple3 = tuple1+tuple2
tuple4 = ('a',(1,2),3,'b',('hard','rock'))
tuple4[1]: (1,2)
tuple4[4][1]: 'rock'
tuple4[4][1][0]: 'r'
```

### Lists

Mutable  
No differences from vector and lists, elements from different types

```python
["Massy", 1984, 33.1, True]
Massy = ["Max", 1984, "M"]    #list saved in variable
Ceci = ["Ceci", 1987, "F"]
Ceci + Massy   #["Max", 1984, "M","Ceci", 1987, "F"]
Ceci*3   #["Ceci", 1987, "F","Ceci", 1987, "F","Ceci", 1987, "F"]

#sublist
famiglia = [["Papà Angelo", 1948, "M"], ["Mamma Giovanna", 1955, "F"], ["Vale", 1980, "F"], Massy, ["Chicco", 1988, "M"], ["Benny", 1996, "F"]]

famiglia[1]  #second element
famiglia[-1]   #second starting from the end
famiglia[3:5]  #first included (3), second not (5), so third and fourth
famiglia[:4]   #from 0 to 4
famiglia[4:]   #from 4 to last one

famiglia[2][1] #second element of the third element

Massy[1]=1985  #modify the element 1
Massy[0:2]=["Massy", 1983]   #modify two elements

del(famiglia2[-1])  #delete last element

1987 in Ceci  #TRUE if 1987 value is in Ceci list
```

### Sets

collections like tuples and lists but unordered and with unique elements

```python
set1 = {'a',1,2,'a',3}
set1: {1,2,3,'a'}
set2 = {'a',4}
set1 & set2: {'a'}
```

### Dictionaries (keys)

collections with name (no duplicates) for each element

```python
x = {"Massi Figini": "@massyfigini", "Primo Drudi": "@primodrudi"}
x.keys()    #get all the keys of x
x.values()    #get all the values of x
x['abacabb'] = 'new'    #add new element to x
del(x['abacabb'])     #delete an element from x

#add key to value
x = {"Massi Figini","@massyfigini", "notebook.massimilianofigini.com"}
nome, twitter, blog= x
```

### Basic Functions

* print(variable)
    
* max(list), min(list)
    
* round(num, decimal places) -&gt; decimal places optional
    
* help(function)
    
* len(string or list)
    
* sum(list) -&gt; sum of list elements
    
* int(numasstring), float(num), str(num), bool(1) -&gt; convertions
    
* complex(real\[, imag\]) -&gt; complex number
    
* sorted(num, reverse=True) -&gt; return sorted elements
    
* split to split string or convert them to lists
    

```python
'Massi Figini'.split(' ')[0]   #return Massi
'Massi Figini'.split()   #return the list ['Massi','Figini']
print('Dybala' + str(21))   #return Dybala21
```

* set(lista) -&gt; convert list to set
    
* File1 = open("percorso/file.txt","r") -&gt; read txt and assign to File1, "r" for reading, "w" for writing and to create a file, "a" for appending
    

```python
#Es.
sells = {
    'price': 10.15,
    'num': 4,
    'customer': 'Pogba'}

stringa = '{} buys {} balls at the price of {} each one for a total of {}'

print(stringa.format(sells['customer'],
             sells['num'],
             sells['price'],
             sells['num']*vendita['prezzo']))
```

The result is "Pogba buys 4 balls at the price of 10.15 each one for a total of 40.6".

### Functions creation

```python
#Function without elements
def yeah():
      print("Yeah!")

#Call the function with:
yeah()

#Addiction function:
def add(x,y):
    z = x+y 
    return z

#Now I can use it with
add(6,7)

#Can add function to a variable
a = add
#Now I can use it with
a(1,3)
```

If I declare a variable inside a function, it exist only in it. If want a global variable define it with the prefix "global".

### If Cycle

Execute indent lines if condition is true

If \[condition\]:  
\[code\]  
elif \[condition2\]:  
\[code\]  
else:  
\[code\]

elif (else if) and else are optional

```python
def add(x,y,z=None):
    if (z==None):
        return x+y
    else:
        return x+y+z
```

### For Loop

```python
#Example 1
for i in range(1,11):
    print(i)
#Example 2
years = [1983,1987,2014]
for i in years:
    print(years)
#Example 3
years = [1983,1987,2014]
for i,years in enumerate(years):
    print(i,years)
```

### While Loop

```python
#Example
dates = [1972,1982,1987]
i = 0
year = 0
while year != 1982:
    year = dates[i]
    i = i+1
    print(year)

print(i, "loops")
```

### With Statement

```python
#Useful example 1: read a file, do something, then close it
with open('/documents/Example1.txt', 'r') as file1:
    FileContent = file1.read()
    print(FileContent)

#Useful example 2: write something i a new file, then close it
with open('/documents/Example2.txt', 'w') as writefile:
    writefile.write("This is line 1\n")
    writefile.write("This is line 2\n")

# Useful example 3: copy Example 2 to Example 3
with open('Example2.txt','r') as readfile:
    with open('Example3.txt','w') as writefile:
          for line in readfile:
                writefile.write(line)
```

### Methods

Functions that belong to objects, different objects have different methods.

```python
famiglia.index("Massy")   #position of Massy in famiglia list
Var.index("a")   #position of a in string variable Var
famiglia.count("Massy")   #how many Massy in famiglia list
famiglia.append(["Ceci", 1987, "F"])   #add sublist in list
famiglia.extend(["Ceci"])   #add element to list
```

Other methods:

* a.capitalize() -&gt; uppercase first letter
    
* a.upper() -&gt; uppercase all
    
* a.find('a') -&gt; find in a string
    
* a.count("o") -&gt; count in string
    
* t.sort() -&gt; sort elements of a list
    

Some methods modified the element that calls them:

* a.replace("o","ops") -&gt; replace in string
    
* t.remove("c") -&gt; remove from list (first one finded)
    
* t.reverse()
    
* set1.add('b')
    
* set1.remove('b')
    
* set1.union(set2)
    
* set1.intersection(set2)
    
* set1.difference(set2) -&gt; in set1 and not in set2
    
* set1.issubset(set2) -&gt; true if subset
    
* File1.name -&gt; name of file assigned to variable File1
    
* File1.mode -&gt; "r" read, "w" write, "a" append
    
* File1.close -&gt; close connection to File1
    

### Class

Class example:

```python
class Persona:
    lavoro = 'Data Scientist'    #class variable
    def set_p(self, nome, luogo):   #method
        self.nome = nome
        self.luogo= luogo

persona = Persona()
persona.set_p('Massi','Milano')
print('{} lives in {} and his dream job is {}'.format(persona.nome, persona.luogo, persona.lavoro))
```

More complex class example:

```python
# Import the library
import matplotlib.pyplot as plt

# Define the class:
class Circle(object):   
    # Constructor
    def __init__(self, radius=3, color='blue'):
        self.radius = radius
        self.color = color     
    # Method
    def add_radius(self, r):
        self.radius = self.radius + r
        return(self.radius)    
    # Method
    def drawCircle(self):
        plt.gca().add_patch(plt.Circle((0, 0), radius=self.radius, fc=self.color))
        plt.axis('scaled')
        plt.show()

# Create an instance of the class:
RedCircle = Circle(10, 'red')

# FInd methods of the class:
dir(RedCircle)

# print radius and color:
RedCircle.radius
RedCircle.color

# Set the object attribute radius
RedCircle.radius = 1
RedCircle.radius

# Call the method drawCircle
RedCircle.drawCircle()

# Use method to change the object attribute radius
print('Radius of object:',RedCircle.radius)
RedCircle.add_radius(2)
print('Radius of object of after applying the method add_radius(2):',RedCircle.radius)
RedCircle.add_radius(5)

print('Radius of object of after applying the method add_radius(5):',RedCircle.radius)

# Create a blue circle with a given radius
BlueCircle = Circle(radius=100)

# Print the object attribute radius
BlueCircle.radius

# Print the object attribute color
BlueCircle.color

# Call the method drawCircle
BlueCircle.drawCircle()
```

### List comprehensions

Ex. function

```python
lista = []
for i in range(1, 1000):
    if i % 2 == 0:
        lista .append(i)
```

Same function with list comprehensions

```python
lista = [i for i in range(0,1000) if i % 2 == 0]
```

### Date and time

```python
import datetime as dt
import time as tm
tm.time()   #seconds from 1/1/1970

#timestamp to datetime
ora = dt.datetime.fromtimestamp(tm.time())

#to take part of datetime:
ora.year, ora.month, ora.day, ora.hour, ora.minute, ora.second
```