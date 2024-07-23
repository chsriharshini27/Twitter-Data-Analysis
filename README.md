# Twitter-Data-Analysis

Setting Up Your Environment:
First, you need to install the necessary libraries. You can do this using pip:
    
    pip install tweepy pandas matplotlib seaborn



     import tweepy

    # Replace these values with your own API keys
      api_key = 'YOUR_API_KEY'
      api_secret_key = 'YOUR_API_SECRET_KEY'
     access_token = 'YOUR_ACCESS_TOKEN'
    access_token_secret = 'YOUR_ACCESS_TOKEN_SECRET'

    auth = tweepy.OAuth1UserHandler(api_key, api_secret_key, access_token, access_token_secret)
      api = tweepy.API(auth)

  Data collection:

         import pandas as pd

    def collect_tweets(query, count=100):
         tweets = tweepy.Cursor(api.search_tweets, q=query, lang="en", tweet_mode='extended').items(count)
         tweets_list = [[tweet.created_at, tweet.user.screen_name, tweet.full_text] for tweet in tweets]
         return pd.DataFrame(tweets_list, columns=['Datetime', 'User', 'Text'])

    df = collect_tweets('#example', count=200)
    df.to_csv('tweets.csv', index=False)

Performing analysis:

        from textblob import TextBlob

    def analyze_sentiment(text):
    analysis = TextBlob(text)
    if analysis.sentiment.polarity > 0:
        return 'Positive'
    elif analysis.sentiment.polarity == 0:
        return 'Neutral'
    else:
        return 'Negative'

    df['Sentiment'] = df['Text'].apply(analyze_sentiment)
    sentiment_counts = df['Sentiment'].value_counts()
    print(sentiment_counts)

Visualizing:

        import matplotlib.pyplot as plt
       import seaborn as sns

      sns.countplot(x='Sentiment', data=df)
     plt.title('Sentiment Analysis of Tweets')
     plt.xlabel('Sentiment')
    plt.ylabel('Number of Tweets')
        plt.show()



