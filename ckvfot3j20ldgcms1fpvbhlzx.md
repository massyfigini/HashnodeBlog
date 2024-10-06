---
title: "AWS in Python with Boto3"
datePublished: Sun Oct 31 2021 20:29:55 GMT+0000 (Coordinated Universal Time)
cuid: ckvfot3j20ldgcms1fpvbhlzx
slug: aws-in-python-with-boto3
tags: data, aws, python

---

To interact with AWS in Python, there is the Boto3 library.

### 1\. AWS S3

S3 is the AWS Storage solution.

```sql
import boto3

# Generate the boto3 client for interacting with S3
s3 = boto3.client('s3', 
     region_name='eu-south-1',    #region where your resources are located
     aws_access_key_id=AWS_KEY,    #your aws key
     aws_secret_access_key=AWS_SECRET)     #your aws secret

# Create a bucket
s3.create_bucket(Bucket='new_bucket_230383')

# Delete a buckets
s3.delete_bucket(Bucket='new_bucket_230383')

# Get the list_buckets response
response = s3.list_buckets()
# Iterate over Buckets from .list_buckets() response
for bucket in response['Buckets']: 
     # Print the Name for each bucket
     print(bucket['Name'])

# Upload file to the bucket
s3.upload_file(Filename='m455y.csv',    #this is the local file path
     Bucket='new_bucket_230383',
     Key='m455y.csv')     #this is the name the object will have in s3

# Get object metadata
s3.head_object(Bucket='new_bucket_230383',
     Key='m455y.csv')

# Download file
s3.download_file(Filename='m455y.csv',    #this is the local path we want to download the file
     Bucket='new_bucket_230383',
     Key='m455y.csv')     #this is the name the object will have in s3

# 1. Access file directly
obj = s3.get_object(Bucket='new_bucket_230383',Key='m455y.csv')

# 2. Then read the file with pandas
pd.read_csv(obj['Body'])   # Body contains the data, the rest are the metadata
```

By default, files in S3 are private.  
In the web console or with some boto methods, you can generate public file or you can temporary shared private files:

```sql
#Temporary shared file with get_object method
share_url = s3.generate_presigned_url(
     ClientMethod='get_object',
     ExpiresIn=3600,
     Params={'Bucket': 'new_bucket_230383', Key: 'm455y.csv'})

#Open Temporary shared files in pandas
pd.read_csv(share_url)
```

### 2\. AWS SNS

SNS = Simple Notification Service.  
With SNS you can send email, text message and push notifications.

```sql
# Initialize the SNS client
sns = boto3.client('sns',
     region_name='eu-central-1',
     aws_access_key_id=AWS_KEY,    #your aws key
     aws_secret_access_key=AWS_SECRET)     #your aws secret

# Create a topic
sns.create_topic(Name='my_alerts')['TopicArn']

# Listing Topics
sns.list_topics()['Topics']

# We can delete topic using its arn
sns.delete_topic(TopicArn='arn:aws:sns:eu-central-1:320333378981:my_alerts'

# Create a new SMS subscription
sns.subscribe(
     TopicArn = 'arn:aws:sns:eu-central-1:320333378981:my_alerts',
     Protocol = 'SMS',
     Endpoint = '+393331234567'

# Create a new email subscription
sns.subscribe(
     TopicArn = 'arn:aws:sns:eu-central-1:320333378981:my_alerts',
     Protocol = 'email',
     Endpoint = 'primodrudi@fakemail.com'

# List topic subscriptions
sns.list_subscriptions_by_topic(TopicArn = 'arn:aws:sns:eu-central-1:320333378981:my_alerts')

# List all topics subscriptions
sns.list_subscriptions()['Subscriptions']

# Delete a subscription
sns.unsubscribe(SubscriptionArn='arn:aws:sns:eu-central-1:320333378981:my_alerts:6b0e71bd-7e97-4d97-80ce-4a0994e55286')

# Publish a topic to send email and sms
sns.publish(
     TopicArn = 'arn:aws:sns:eu-central-1:320333378981:my_alerts',
     Message = 'Hello world!',
     Subject = 'Hi')    #subject is visible only for email

# Send a single SMS (don't need topic and subscriptions)
sns.publish(
     PhoneNumber = '+393331234567'
     Message = 'Hello world!')
```

### 3\. AWS REKOGNITION

Computer vision API by AWS.

```sql
# Initialize the S3 client
s3 = boto3.client('s3',
     region_name='eu-central-1',
     aws_access_key_id=AWS_KEY,    #your aws key
     aws_secret_access_key=AWS_SECRET)     #your aws secret

# Upload a file
s3.upload_file(Filename='file1.jpg', Key='file1.jpg', Bucket='m455y-img')

# Initialize the Rekognition client
rekog = boto3.client('rekognition',
     region_name='eu-central-1',
     aws_access_key_id=AWS_KEY,
     aws_secret_access_key=AWS_SECRET)

# Object recognition
rekog.detect_labels(
     Image={'S3Object': {
          'Bucket': 'm455y-img',
          'Name': 'file1.jpg'},
     MaxLabels=10,     #optional max amount of labels to return
     MinConfidence=95})     #optional min accettable confidence in the match

# Text detenction
rekog.detect_labels(
     Image={'S3Object': {
          'Bucket': 'm455y-img',
          'Name': 'file1.jpg'}})
```

### 4\. AWS TRANSLATE

Real-time translation service by AWS.

```sql
# Initialize the Translate client
translate = boto3.client('translate',
     region_name='eu-central-1',
     aws_access_key_id=AWS_KEY,    #your aws key
     aws_secret_access_key=AWS_SECRET)     #your aws secret

# Translate
translate.translate_text(
     Text='Ciao, come stai?',
     SourceLanguageCode='auto',
     TargetLanguageCode='en')['Translated_Text']
```

### 5\. AWS COMPREHEND

Text comprehention service by AWS.

```sql
# Initialize the Comprehend client
comprehend = boto3.client('comprehend',
     region_name='eu-central-1',
     aws_access_key_id=AWS_KEY,    #your aws key
     aws_secret_access_key=AWS_SECRET)     #your aws secret

# Detect language
comprehend.detect_dominant_language(
     Text=''Ciao, come stai? Io bene, grazie.")

# Detect text sentiment
comprehend.detect_sentiment(
     Text=''This article is really good.",
     LanguageCode='en')
```