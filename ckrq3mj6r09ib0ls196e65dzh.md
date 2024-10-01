---
title: "Word Clouds with Python"
datePublished: Sun Jul 21 2019 08:41:11 GMT+0000 (Coordinated Universal Time)
cuid: ckrq3mj6r09ib0ls196e65dzh
slug: word-clouds-with-python
tags: python, data-visualization

---

```sql
import numpy as np

from PIL import Image     #converting images into arrays
import matplotlib as mpl
import matplotlib.pyplot as plt

#import package and its set of stopwords
from wordcloud import WordCloud, STOPWORDS

#remove stopwords
stopwords = set(STOPWORDS)

#we can add any word that we don't want visualize
stopwords.add('word')

#open the file and read it into a variable
words = open('words.txt', 'r').read()

#instantiate a word cloud object
new_wc = WordCloud(
    background_color='white',
    max_words=2000,
    stopwords=stopwords
)

#generate the word cloud
new_wc.generate(words)

#display the word cloud
plt.imshow(new_wc, interpolation='bilinear')
plt.axis('off')
plt.show()

#we can resize it
fig = plt.figure()
fig.set_figwidth(14)     # set width
fig.set_figheight(18)     # set height

#display the cloud
plt.imshow(new_wc, interpolation='bilinear')
plt.axis('off')
plt.show()
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627634455543/BDetpGqgS.png align="left")

```sql
#we can use an image as background instead the rectangle when instantiate the object
new_img = np.array(Image.open('image.png'))

new_wc = WordCloud(
    background_color='white',
    max_words=2000,
    mask=new_img,
    stopwords=stopwords
)
```