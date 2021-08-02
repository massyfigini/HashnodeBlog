## Python error handling

### ERROR HANDLING WITH TRY-EXCEPT

```
def echo(word, e=1):
    """Concatenate echo copies of a word"""

    try:
        echo_word = word * e

    except:
        print("word must be a string and echo must be an integer.")

    return echo_word 
``` 

Call the function
``` 
echo("hello", 5)
``` 

### ERROR HANDLING WITH RAISE
``` 
# Define shout_echo
def echo(word, e=1):
    """Concatenate echo copies of a word"""

    # Raise an error with raise
    if e < 0:
        raise ValueError('e must be greater than 0')

    echo_word = word * e

    return echo_word 
``` 
Call the function
``` 
echo("hello", echo=5)
``` 