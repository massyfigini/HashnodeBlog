---
title: "A Python Twitter BOT example: Covid-19 BOT Italia"
datePublished: Sun Jan 17 2021 16:49:15 GMT+0000 (Coordinated Universal Time)
cuid: ckrp3p1rx01mdzcs1fny058s3
slug: a-python-twitter-bot-example-covid-19-bot-italia
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690487102638/7c48bec7-1cf2-4ced-899f-2fb6d1858b0c.png
tags: bot, python, bots, python-projects

---

In the previous post ([https://massyfigini.hashnode.dev/how-to-build-and-schedule-a-python-twitter-bot](https://massyfigini.hashnode.dev/how-to-build-and-schedule-a-python-twitter-bot)) you can find how to create your Twitter developer account and how to schedule a Twitter bot.

Here there is the complete code of my "Covid-19 BOT Italia", a BOT started in April and still active, that automatically retweets all the Covid-19 related tweets by Italian authorities.  
You can see the result here: [https://twitter.com/COVIDbot\_ITA](https://twitter.com/COVIDbot_ITA)

You can also find the always updated code on my Github ([https://github.com/massyfigini/CovidBOT](https://github.com/massyfigini/CovidBOT)).

First I have created a CSV file with two fields: Account contains all the Twitter accounts of the Italian authorities (ex. GiuseppeConteIT is the account name of the Italian Prime Minister), LastTweetID contains the ID of the last tweet (numeric, the last part of the tweet link).  
Then I have created an empty txt file named "Error.txt".  
Both files are in the same directory of the Python script.  
This is the Python script:

```sql
C_KEY = "Replace this sentence with your API Key"    
C_SECRET = "Replace this sentence with your API Key Secret"  
A_TOKEN = "Replace this sentence with your Access Token"  
A_TOKEN_SECRET = "Replace this sentence with your Access Token Secret"  

#pip install tweepy

# import libraries
import tweepy
import pandas as pd

# Authenticate to Twitter
auth = tweepy.OAuthHandler(C_KEY, C_SECRET)
auth.set_access_token(A_TOKEN, A_TOKEN_SECRET)

# Create API object
api = tweepy.API(auth)

# search for covid
text1 = 'coronavirus'
text2 = 'covid'
text3 = 'lockdown'
text4 = 'pandemia'

text = [text1, text2, text3, text4]

# read file
df = pd.read_csv('account.csv')

# for each row of my csv
for index, row in df.iterrows():
    
    # take data
    account = row['Account']
    last_tweet_id = row['LastTweetID']
    
    # tweet limit
    max_tweets = 1000
    
    try:
        # take all user tweets
        searched_tweets = [status.id for status in 
                        tweepy.Cursor(api.user_timeline, id = account, since_id = last_tweet_id,
                                        exclude_replies = True).items(max_tweets)]
    except:
        # append in error file
        Err = open("Error.txt", "a", encoding="utf-8")
        Err.write(" ERROR ACCOUNT: " + account + "\n")
        raise
    
    try:
        # for each tweet
        for idx, i in enumerate(searched_tweets):
            tweet = api.get_status(i, tweet_mode = 'extended')  # all the text
            tweetText = tweet.full_text   # take text
            tweetText = tweetText.lower()   # lowercase
            if idx == 0: df.iloc[index,3] = tweet.id   # last retweeted id
            # filter only covid tweets and exclude retweets
            if not(tweetText.startswith('rt')) and any(x in tweetText for x in text):
                # tweet!    
                api.retweet(i)
             
    except:
        # append in error file
        Err = open("Error.txt", "a", encoding="utf-8")
        Err.write("ERROR TWEET: " + tweetText + "\n")
        raise

# rewrite account.csv with last id tweet updated
df.to_csv('account.csv', index=False)
```

As you can see, for each account in the csv file, the script retweets every tweet that contains some key words starting with the last tweet ID. After that, it updates the csv file with the ID of the last tweet. If there is an error, the tweet that generates it is written in the txt file, so you can easily investigate the problem.