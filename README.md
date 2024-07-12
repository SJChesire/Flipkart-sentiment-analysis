Flipkart is one of the most popular Indian companies. It is an e-commerce platform that competes with popular e-commerce platforms like Amazon.
Sentiment analysis is the process of analyzing digital text to determine if the emotional tone of the message is positive, negative, or neutral
Here we learn how to analyze the sentiment of Flipkart reviews, of product reviews sold on the e-commerce platform. 
In this article, I will walk you through the task of Flipkart reviews sentiment analysis using Python.

The dataset I am using here for Flipkart reviews sentiment analysis is downloaded from Kaggle. 
STEP 1: Importing the necessary Python libraries and the dataset
Then, Check whether any of the columns contains null values or not by using print(data.isnull().sum())

STEP 2: Prepare and clean the data
I will clean and prepare the column containing reviews before heading to sentiment analysis

import nltk
import re
nltk.download('stopwords')
stemmer = nltk.SnowballStemmer("english")
from nltk.corpus import stopwords
import string
stopword=set(stopwords.words('english'))

def clean(text):
    text = str(text).lower()
    text = re.sub('\[.*?\]', '', text)
    text = re.sub('https?://\S+|www\.\S+', '', text)
    text = re.sub('<.*?>+', '', text)
    text = re.sub('[%s]' % re.escape(string.punctuation), '', text)
    text = re.sub('\n', '', text)
    text = re.sub('\w*\d\w*', '', text)
    text = [word for word in text.split(' ') if word not in stopword]
    text=" ".join(text)
    text = [stemmer.stem(word) for word in text.split(' ')]
    text=" ".join(text)
    return text
data["Review"] = data["Review"].apply(clean)

STEP 3: Carry out sentiment analysis
Let us check out how people review the products they purchase from flipkart
This will be displayed in a chart

ratings = data["Rating"].value_counts()
numbers = ratings.index
quantity = ratings.values

import plotly.express as px
figure = px.pie(data, 
             values=quantity, 
             names=numbers,hole = 0.5)
figure.show()

STEP 4: 
Now let’s have a look at the kind of reviews people leave on their purchases. 
I will use a word cloud to visualize the most used words in the reviews column
text = " ".join(i for i in data.Review)
stopwords = set(STOPWORDS)
wordcloud = WordCloud(stopwords=stopwords, 
                      background_color="white").generate(text)
plt.figure( figsize=(15,10))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.show()

STEP 5: 
Now let’s see how most of the reviewers think about the purchases from Flipkart ranging from Positive, neutral and negative

