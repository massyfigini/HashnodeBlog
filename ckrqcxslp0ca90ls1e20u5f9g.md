## Python Lambda functions

You can write small anonymous function with Lambda:

```
exclamation = (lambda a : a + '!')
exclamation('hello')

echo = (lambda word, e: world*e)
echo('hello',5)
``` 


**map() with lambda functions**  
map() applies a function over an object
``` 
l = ['a', 'b', 'c', 'd']
exclamation2 = map(lambda a : a+'!', l)
print(list(exclamation2))
``` 

**filter() with lambda functions**  
filter() is to filter out elements from a list that don't satisfy certain criteria
``` 
l = ['hello', 'hi', 'ciao', 'hola', 'marhabaan', 'hallo', 'alo']
longer = filter(lambda a : len(a) > 4, l)
print(list(longer))
``` 

**reduce() with lambda functions**  
reduce() is useful for performing some computation on a list and returns a single value
``` 
from functools import reduce
l = ['hello', 'hi', 'ciao', 'hola', 'marhabaan', 'hallo', 'alo']
result = reduce(lambda a, b : a + b, l)
print(result)
``` 