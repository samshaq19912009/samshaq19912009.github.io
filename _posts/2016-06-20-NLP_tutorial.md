---
layout: post
title: NLP tutorial: 电影评论情感分析
date: 2016-06-20
categories: blog
tags: [Machine Learning,Notes]
description: NLP
use_math: true
---

# NLP tutorial: 电影评论情感分析


自然语言处理是用来处理语言文字的工具，目的是为了把自然語言轉化為計算機程序更易于處理的形式。这一篇tutorial是用来分析电影评论的情感（sentimental anaylysis）。

## 读入数据

首先需要录入data， train data有三行，id, sentiment(0 or 1)和reviews, 目的是根据review来预测sentiment。


### 数据清理

根据每一行的review，可以发现里面存在很多HTML的tag，这些tag需要去除。这里可以用BeautifulSoup来清理

```Python
# Import BeautifulSoup into your workspace
from bs4 import BeautifulSoup             

# Initialize the BeautifulSoup object on a single movie review     
example1 = BeautifulSoup(train["review"][0])  

# Print the raw review and then the output of get_text(), for 
# comparison
print train["review"][0]
print example1.get_text()
```

这样可以得到纯文字的文本。进行下一步的处理

### 处理数字，标点

在很多自然语言分析中，数字和标点符号可以有不同的含义。例如，感叹号可以在分析情感中起到作用，这里出于简化，我们把非字母的字符都去掉，可以使用正则表达式（regular expression)

```Python
import re
# Use regular expressions to do a find-and-replace
letters_only = re.sub("[^a-zA-Z]",           # The pattern to search for
                      " ",                   # The pattern to replace it with
                      example1.get_text() )  # The text to search
print letters_only

```
最后，我们再加入stop words，一些常见的单词不计入最后的数据

```Python
# Remove stop words from "words"
words = [w for w in words if not w in stopwords.words("english")]
print words
```

## Bag of words 

Bag of words model: 
从所有的文档出提取出一个词汇表（vocabulary）， 然后对每一个文档统计每一个单词出现的频率。例如以下两个句子：

Sentence 1: "The cat sat on the hat"

Sentence 2: "The dog ate the cat and the hat"

根据以上两个句子，所得的vocabulary为：

{ the, cat, sat, on, hat, dog, ate, and }

这样， 每一个句子就转换为一个向量，

Sentence 1: { 2, 1, 1, 1, 1, 0, 0, 0 }

Sentence 2：{ 3, 1, 0, 0, 1, 1, 1, 1}

在这里处理电影评论的时候，由于总体vocabulary量过大，只选用最长出现的500个单词，利用scikit-learn的 [CountVectorizer](http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html).具体代码如下：

```Python
print "Creating the bag of words...\n"
from sklearn.feature_extraction.text import CountVectorizer

# Initialize the "CountVectorizer" object, which is scikit-learn's
# bag of words tool.  
vectorizer = CountVectorizer(analyzer = "word",   \
                             tokenizer = None,    \
                             preprocessor = None, \
                             stop_words = None,   \
                             max_features = 5000) 

# fit_transform() does two functions: First, it fits the model
# and learns the vocabulary; second, it transforms our training data
# into feature vectors. The input to fit_transform should be a list of 
# strings.
train_data_features = vectorizer.fit_transform(clean_train_reviews)

# Numpy arrays are easy to work with, so convert the result to an 
# array
train_data_features = train_data_features.toarray()

```
如果想要得到vocabulary的具体feature，或者分析某一个Word在所有文档里出现的频率，可以使用如下代码

```Python
# Take a look at the words in the vocabulary
vocab = vectorizer.get_feature_names()
print vocab

# Sum up the counts of each vocabulary word
dist = np.sum(train_data_features, axis=0)

# For each, print the vocabulary word and the number of times it 
# appears in the training set
for tag, count in zip(vocab, dist):
    print count, tag

```

## Random Forest 

现在对于每一个review， 已经转换成了一个向量，接下来，使用[Random Forest](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)的方法来进行分类。

```Python
print "Training the random forest..."
from sklearn.ensemble import RandomForestClassifier

# Initialize a Random Forest classifier with 100 trees
forest = RandomForestClassifier(n_estimators = 100) 

# Fit the forest to the training set, using the bag of words as 
# features and the sentiment labels as the response variable
#
# This may take a few minutes to run
forest = forest.fit( train_data_features, train["sentiment"] )

```

训练好model后，就可以录入test data，重复之前我们对train data的pre-processing，再predict label

```Python
# Read the test data
test = pd.read_csv("testData.tsv", header=0, delimiter="\t", \
                   quoting=3 )

# Verify that there are 25,000 rows and 2 columns
print test.shape

# Create an empty list and append the clean reviews one by one
num_reviews = len(test["review"])
clean_test_reviews = [] 

print "Cleaning and parsing the test set movie reviews...\n"
for i in xrange(0,num_reviews):
    if( (i+1) % 1000 == 0 ):
        print "Review %d of %d\n" % (i+1, num_reviews)
    clean_review = review_to_words( test["review"][i] )
    clean_test_reviews.append( clean_review )

# Get a bag of words for the test set, and convert to a numpy array
test_data_features = vectorizer.transform(clean_test_reviews)
test_data_features = test_data_features.toarray()

# Use the random forest to make sentiment label predictions
result = forest.predict(test_data_features)

# Copy the results to a pandas dataframe with an "id" column and
# a "sentiment" column
output = pd.DataFrame( data={"id":test["id"], "sentiment":result} )

# Use pandas to write the comma-separated output file
output.to_csv( "Bag_of_Words_model.csv", index=False, quoting=3 )

```

## Word Vector Model

接下来使用 word vector model,将每一个Word转换成一个vector，数据清理阶段与上一个model基本相同。

注意的是，这时候需要的是sentence！！！！所以不能直接把某一段paragraph直接换成Wordlist！！！！！

每一个review是一段paragraph，先把paragraph转换成每一个句子，使用**tokenizer.tokenize**

```Python


# Define a function to split a review into parsed sentences
def review_to_sentences( review, tokenizer, remove_stopwords=False ):
    # Function to split a review into parsed sentences. Returns a 
    # list of sentences, where each sentence is a list of words
    #
    # 1. Use the NLTK tokenizer to split the paragraph into sentences
    
    raw_sentences = tokenizer.tokenize(unicode(review.strip(),errors='ignore'))##convert paragraph into sentences
    
    #
    # 2. Loop over each sentence
    sentences = []
    for raw_sentence in raw_sentences:
        # If a sentence is empty, skip it
        if len(raw_sentence) > 0:
            # Otherwise, call review_to_wordlist to get a list of words
            sentences.append( review_to_wordlist( raw_sentence, \
              remove_stopwords ))
    #
    # Return the list of sentences (each sentence is a list of words,
    # so this returns a list of lists
    return sentences



```

### Training and Saving

使用Python的genism package进行word2Vec训练，基本参数为

>**Context / window size** 每次训练时的context size？
>
>
>**Word vector dimensionality** word vector的长度
>
>
>**Downsampling of frequent words**：对常见Word的downsampling，提高模型准确率
>
>**Minimum word count**：减少最终vocabulary的Word数目



```Python

# Import the built-in logging module and configure it so that Word2Vec 
# creates nice output messages
import logging
logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s',\
    level=logging.INFO)

# Set values for various parameters
num_features = 300    # Word vector dimensionality                      
min_word_count = 40   # Minimum word count                        
num_workers = 4       # Number of threads to run in parallel
context = 10          # Context window size                                                                                    
downsampling = 1e-3   # Downsample setting for frequent words

# Initialize and train the model (this will take some time)
from gensim.models import word2vec
print "Training model..."
model = word2vec.Word2Vec(sentences, workers=num_workers, \
            size=num_features, min_count = min_word_count, \
            window = context, sample = downsampling)

# If you don't plan to train the model any further, calling 
# init_sims will make the model much more memory-efficient.
model.init_sims(replace=True)

# It can be helpful to create a meaningful model name and 
# save the model for later use. You can load it later using Word2Vec.load()
model_name = "300features_40minwords_10context"
model.save(model_name)

```

模型训练完毕后，可以用模型来预测，模拟。例子如下

```Python
#哪一个单词和以下最不接近？

model.doesnt_match("man woman child kitchen".split())

#输出‘kitchen’


model.doesnt_match("france england germany berlin".split())

#输出‘Berlin’

#哪些单词最接近？
model.most_similar("man")

#输出
[(u'woman', 0.6092387437820435),
 (u'lad', 0.6064352989196777),
 (u'lady', 0.57511305809021),
 (u'monk', 0.5583359599113464),
 (u'sailor', 0.534398078918457),
 (u'men', 0.5196300745010376),
 (u'farmer', 0.5171995162963867),
 (u'person', 0.5137056112289429),
 (u'guy', 0.501788854598999),
 (u'millionaire', 0.4980509281158447)]
```
此外，可以看到，某一个单词的vector length都是300.

```Python
len(model['computer'])

#输出300

```

## More fun with Word Vector

### From Words To Paragraphs, Attempt 1: Vector Averaging

对于每一篇review， 长度都不固定，所以对于每一篇review，需要将每一个Word vector转换成段落的feature。

给定一段review，计算平均的Wordvector， 以下code中， mode.index2word输出原始的Word dictionary。

```Python
def makeFeatureVec(words, model, num_features):
    # Function to average all of the word vectors in a given
    # paragraph
    #
    # Pre-initialize an empty numpy array (for speed)
    featureVec = np.zeros((num_features,),dtype="float32")
    #
    nwords = 0.
    # 
    # Index2word is a list that contains the names of the words in 
    # the model's vocabulary. Convert it to a set, for speed 
    index2word_set = set(model.index2word)
    #
    # Loop over each word in the review and, if it is in the model's
    # vocaublary, add its feature vector to the total
    for word in words:
        if word in index2word_set: 
            nwords = nwords + 1.
            featureVec = np.add(featureVec,model[word])
    # 
    # Divide the result by the number of words to get the average
    featureVec = np.divide(featureVec,nwords)
    return featureVec
```

在这段code的基础上，给定很多review，输出平均的Word vector。

```Python
def getAvgFeatureVecs(reviews, model, num_features):
    # Given a set of reviews (each one a list of words), calculate 
    # the average feature vector for each one and return a 2D numpy array 
    # 
    # Initialize a counter
    counter = 0.
    # 
    # Preallocate a 2D numpy array, for speed
    reviewFeatureVecs = np.zeros((len(reviews),num_features),dtype="float32")
    # 
    # Loop through the reviews
    for review in reviews:
       #
       # Print a status message every 1000th review
       if counter%1000. == 0.:
           print "Review %d of %d" % (counter, len(reviews))
       # 
       # Call the function (defined above) that makes average feature vectors
       reviewFeatureVecs[counter] = makeFeatureVec(review, model, \
           num_features)
       #
       # Increment the counter
       counter = counter + 1.
    return reviewFeatureVecs


```


根据以上的代码，可以计算把train data 和 test data转换成clean的review

```Python
# ****************************************************************
# Calculate average feature vectors for training and testing sets,
# using the functions we defined above. Notice that we now use stop word
# removal.

clean_train_reviews = []
for review in train["review"]:
    clean_train_reviews.append( review_to_wordlist( review, \
        remove_stopwords=True ))

trainDataVecs = getAvgFeatureVecs( clean_train_reviews, model, num_features )

print "Creating average feature vecs for test reviews"
clean_test_reviews = []
for review in test["review"]:
    clean_test_reviews.append( review_to_wordlist( review, \
        remove_stopwords=True ))

testDataVecs = getAvgFeatureVecs( clean_test_reviews, model, num_features )

```

让我们来检查一下最后得到的trainDataVecs和testDataVects的dimension。可以看到trainDataVecs的dimension为（25000， 300），意为我们有25000个training example，每个example为一个300维的word vector；同理, testDataVecs的dimension也为(25000， 300）。



和bag of word一样，我们可以采用random forest的方法去训练model，这里不再重复。

### From Words To Paragraphs, Attempt 2: Clustering

Word2Vec将会输出相似语义词汇的cluster， 所以，另一个常用方法就是利用同一cluster之间词汇的相似性。 将向量按此分类叫做**Vector quantization**.首先，利用[K-means algorithm](http://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)来确定每一个cluster的center。


```Python
from sklearn.cluster import KMeans
import time

start = time.time() # Start time

# Set "k" (num_clusters) to be 1/5th of the vocabulary size, or an
# average of 5 words per cluster
word_vectors = model.syn0
num_clusters = word_vectors.shape[0] / 5

# Initalize a k-means object and use it to extract centroids
kmeans_clustering = KMeans( n_clusters = num_clusters )
idx = kmeans_clustering.fit_predict( word_vectors )

# Get the end time and print how long the process took
end = time.time()
elapsed = end - start
print "Time taken for K Means clustering: ", elapsed, "seconds."

```

由于cluster的数目过多，以上code需要很长时间来进行训练（在我的电脑上花了24分钟！），idx会是每一个word_vector对应的cluster number。定义一个dictionary，把每一个word map到index number上。

```Python
# Create a Word / Index dictionary, mapping each vocabulary word to
# a cluster number                                                                                            
word_centroid_map = dict(zip( model.index2word, idx ))

```
根据以上的dictionary，我们可以把每一条review转换成bag of centroids.

```Python
def create_bag_of_centroids( wordlist, word_centroid_map ):
    #
    # The number of clusters is equal to the highest cluster index
    # in the word / centroid map
    num_centroids = max( word_centroid_map.values() ) + 1
    #
    # Pre-allocate the bag of centroids vector (for speed)
    bag_of_centroids = np.zeros( num_centroids, dtype="float32" )
    #
    # Loop over the words in the review. If the word is in the vocabulary,
    # find which cluster it belongs to, and increment that cluster count 
    # by one
    for word in wordlist:
        if word in word_centroid_map:
            index = word_centroid_map[word]
            bag_of_centroids[index] += 1
    #
    # Return the "bag of centroids"
    return bag_of_centroids

```
具体分析为，现在number of cluster为3295，对每一条review，转换为3295维的向量，此向量的i位置对应为，出现在第i个cluster的word的数目。

对每一条train和test的review，我们都可以做次变化，得到train_centroids和test_centroids。

```Python
# Pre-allocate an array for the training set bags of centroids (for speed)
train_centroids = np.zeros( (train["review"].size, num_clusters), \
    dtype="float32" )

# Transform the training set reviews into bags of centroids
counter = 0
for review in clean_train_reviews:
    train_centroids[counter] = create_bag_of_centroids( review, \
        word_centroid_map )
    counter += 1

# Repeat for test reviews 
test_centroids = np.zeros(( test["review"].size, num_clusters), \
    dtype="float32" )

counter = 0
for review in clean_test_reviews:
    test_centroids[counter] = create_bag_of_centroids( review, \
        word_centroid_map )
    counter += 1

```

我们来检查一下train_centroids.shape，结果为(25000, 3295)，意为有25000个train sample， 每个sample为3295维的向量。比较之前的train sample维度为(25000, 300)。显然，cluster的方法保留了更多原有信息，也使得模型变得更加复杂。

同理，我们可以用Random Forest进行训练，与之前的code比较，此结果略差于bag of words model。


## Conclusion

为什么利用word2vec得到的结果与之前的bag of words模型类似？最大的原因在于，利用average或者k-means methods，我们已经忽略了word的**order**， 这样，这与bag of words的模型假设已经非常类似。

一些提高的建议：

1.使用更多的text来训练word2vec model。
2.使用**Paragraph Vector**
