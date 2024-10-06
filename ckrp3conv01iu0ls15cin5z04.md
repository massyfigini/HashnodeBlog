---
title: "How to build and schedule a Python Twitter BOT"
datePublished: Wed Jan 06 2021 16:42:27 GMT+0000 (Coordinated Universal Time)
cuid: ckrp3conv01iu0ls15cin5z04
slug: how-to-build-and-schedule-a-python-twitter-bot
tags: tutorial, bot, python, bots

---

### 1\. Create a Twitter developer account

With this account, you have access to the Twitter API we are going to need to develop the bot. Go to [https://developer.twitter.com/en/apply-for-access](https://developer.twitter.com/en/apply-for-access) . Login to your Twitter account and choose "apply for developer account". Choose "Making a bot", answer the questions, accept the Developer Agreement and submit the application.

### 2\. Write the Python script

In the developer portal ( [https://developer.twitter.com/en/portal/dashboard](https://developer.twitter.com/en/portal/dashboard) ), in the "Overview" of the "Projects & apps" section, choose "Create app", and give it a name. Now you can find your keys and tokens to program the app. With your source-code editor, write the code for your bot. You can use the Tweepy package to access almost all Twitter functionality in Python. To interact with your account, you must use this code:

C\_KEY = "Replace this sentence with your API Key"  
C\_SECRET = "Replace this sentence with your API Key Secret"  
A\_TOKEN = "Replace this sentence with your Access Token"  
A\_TOKEN\_SECRET = "Replace this sentence with your Access Token Secret"

```sql
#import libraries (if you haven't installed it use "pip install tweepy")
import tweepy

#Authenticate to Twitter
auth = tweepy.OAuthHandler(C_KEY, C_SECRET)
auth.set_access_token(A_TOKEN, A_TOKEN_SECRET)
```

In my next post I'll give you an example of a complete bot code.

### 3\. Schedule the bot with Task Scheduler

The simplest way to schedule a bot on a Windows PC is through Task Scheduler. First we have to create a bat file to run the Python script. Open a notepad file and paste this code:

```sql
"C:\[Dir1]\python.exe" "C:\[Dir2]\COVID_TwitterBOT\bot.py"
```

In the code above, replace \[Dir1\] with the path of your Python installation, \[Dir2\] with the path of your Python script, and "bot.py" with the name of your Python script. Save the text file in the same directory of the Python script and name it "Run\_BOT.bat". If you double click the file, the Python script will be executed. NB: if you get an error, it's possible you have to add Python to the Windows Path, you can find a guide at [https://geek-university.com/python/add-python-to-the-windows-path/](https://geek-university.com/python/add-python-to-the-windows-path/) .

Now enter Task Scheduler and on the left, under "Task Scheduler Library" right click and choose "Create Task...". In the "General" tab, give it a name and choose "Run whether user is logged or not". In the "Triggers" tab, click on "New..." and choose the trigger type. For example if you need to run the script every 5 minutes, choose "Repeat task every" and select "5 minutes" and "Indefinitely" in the duration box. Make sure to tick "Enabled" in the last checkbox, and choose OK. In the "Actions" tab, click on "New...", choose "Start a Program" in the first box, in the Program/Script box click on "Browse" and select your script, in the "Add arguments" box write the name of your bat file (Run\_BOT.bat), and in the "Start in" box write the entire path in which you have the bat file without the file name (es. C:\\BotProject). Click ok and then right click on the new task you have just created and choose "Run" for the first run. Now your script will be execute every 5 minutes when your PC is turned on.