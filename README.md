# Twitter-Scraping-For-Latest-Cloud-News

    import tweepy
    import csv
    import os
    import time
    import datetime
    import sys
    import re
    from datetime import timezone

    # import pandas as pd
    ####input your credentials here
    # gui part

    consumer_key = 'xxxxxxxxxxxxxxxxxxxxxxxx'
    consumer_secret = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
    access_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
    access_token_secret = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

    auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    api = tweepy.API(auth, wait_on_rate_limit=True)
    csvFile = open('C:/Csha/test.csv', 'w',newline='')
    csvWriter = csv.writer(csvFile)
    csvWriter.writerow(["Tweet Text", "Number of Retweets", "Tweet Id", "User Name", "User Id", "User Followers Count","Search String", "Unix Timestamp","Number of Likes for tweet",'All hashtags in tweet'])

    with open('C:/Csha/input.csv', newline='') as f:
        reader = csv.reader(f)
        temp_keywords = list(reader)
    keywords=[item for sublist in temp_keywords for item in sublist]
    print(keywords)


    for i in keywords:
        for tweet in tweepy.Cursor(api.search, q=i, lang="en", count=1000).items(9):
            print(tweet.text, tweet.retweet_count, tweet.id_str, tweet.user.name,
                  tweet.user.id_str, "\n", i, tweet.created_at,tweet.user.followers_count,tweet.favorite_count)
            all_hash_tags = re.findall(r"#(\w+)", tweet.text)
            final_hash_tags=','.join(all_hash_tags)
            print('\n')
            unix_ts_utc = tweet.created_at.replace(tzinfo=timezone.utc).timestamp()
            csvWriter.writerow([re.sub(r'\bRT\b','',tweet.text), tweet.retweet_count,tweet.id_str, tweet.user.name,
                                tweet.user.id_str, tweet.retweet_count, i, unix_ts_utc,tweet.user.followers_count,tweet.favorite_count,final_hash_tags])
