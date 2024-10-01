---
title: "Python error handling"
datePublished: Sun Sep 02 2018 12:38:08 GMT+0000 (Coordinated Universal Time)
cuid: ckrqcl1yv0c6gzcs12pbi5o6n
slug: python-error-handling
tags: python, error-handling, debugging

---

### ERROR HANDLING WITH TRY-EXCEPT

```sql
def echo(word, e=1):
    """Concatenate echo copies of a word"""

    try:
        echo_word = word * e

    except:
        print("word must be a string and echo must be an integer.")

    return echo_word
```

Call the function

```sql
echo("hello", 5)
```

### ERROR HANDLING WITH RAISE

```sql
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

```sql
echo("hello", echo=5)
```