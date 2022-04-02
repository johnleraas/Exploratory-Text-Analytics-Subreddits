


# Exploratory-Text-Analytics-Subreddits
Implementation of various NLP techniques to compare and evaluate a set of 20 subreddits

## Data Description
* A subset of 20 subreddits of an initial 1,103 was selected

## Data Cleaning, Preparation, and Organization
* Data cleaning (e.g. line breaks, contractions, html tags) performed prior to tokenization
* Ordered Hierarchy of Content Objects (OHCO) defined as ['category', 'subreddit', 'post_id', 'para_num', 'sent_num', 'token_num']
* Parts of Speech tagging added utilizing NLTK
* Porter stemmer implemented with NLTK
* Emotion and Sentinment added with NRC Lexicon: https://saifmohammad.com/WebPages/NRC-Emotion-Lexicon.htm
* Vector Representation achieved with TF-IDF at the "subreddit" and "category" level

# Principal Component Analysis (PCA)

<p align="center">
  <img height=50% width=50% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/Scree_Plot.png">
</p>

Given the computational requirements to apply TF-IDF to each post, PCA was conducted with the subreddit OHCO level selected for the bag of words. The subreddit document context matrix was normalized for length, but not variance in order to avoid exaggerating the importance of rare words.

The resulting Scree plot indicates that of the 20 principal components, 19 explain fairly similar portions of the variance. This makes some intuitive sense, as there are 20 unique subreddits in the dataset. 

<p align="center">
  <img height=45% width=45% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/PC0_PC1.png"> 
  <img height=45% width=45% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/PC6_PC7.png">
</p>

The importance of each principal component was also highlighted by plotting different principal components on different two-dimensional axes. For example, as seen below, the first or second principal components do not describe the data meaningfully better than the sixth or seventh components.

Principal Component Analysis indicates that most of the subreddits possess characteristics that make them unique.

# Topic Modeling

Topic modeling was performed by extracting and aggregating the nouns from each post, applying a CountVectorizer from sklearn, and then applying sklearn’s LatentDirichletAllocation to decompose the matrix into a document-topic matrix and a topic-term matrix. Modeling with different numbers of topics were explored, but ‘five’ was eventually chosen as a fairly interpretable selection.

<p align="center">
  <img height=75% width=75% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/Topics_by_Category.png">
</p>

<p align="center">
  <img height=75% width=75% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/Topics_by_Subreddit.png">
</p>

As observed above:
* Topic 1 is largely associated with video games
* The categories ‘hobby’ and ‘electronics’ are largely associated with Topic 2, however the underlying ‘camping’ subreddit has the lowest proportion of Topic 2
* The categories ‘tv_show’ and ‘music’ are largely composed of Topic 3
* The ‘software’ and ‘crypto’ categories (‘btc’, ‘AdobeIllustrator’, ‘salesforce’, ‘dogecoin’) are composed primarily of Topic 4
* Topic 5 is largely associated with sports
* The category ‘geo’ and underlying subreddits ‘Norway’ and ‘france’ are fairly balanced by topic

# Sentiment Analysis

Sentiment was explored using two tools and techniques, namely the NRC Word-Emotion Lexicon and Valence Aware Dictionary for sEntiment Reasoning (VADER). First, sentiment and emotion were explored with use of the NRC Word-Emotion Lexicon which provides lexicons for positive and negative sentiment as well as the following emotions: anger, anticipation, disgust, fear, joy, sadness, surprise, and trust. Select emotions with ‘negative’ connotations (anger, fear, disgust) and ‘positive’ connotations (joy, trust, anticipation) are presented below.

<p align="center">
  <img height=50% width=50% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/Neg_Emotion.png">
</p>
<p align="center">
  <img height=50% width=50% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/Pos_Emotion.png">
</p>

Interestingly, the subreddits most closely associated with anger/fear/disgust are generally characterized by ‘ProtectandServe’ (law enforcement personnel), ‘btc’ (bitcoin), and ‘FORTnITE’ (the video game). The subreddits most closely associated with joy/trust/anticipation are ‘beatles’ (the band), ‘TaylorSwift’ (the singer), and ‘FORTnITE’. Notably, ‘ProtectandServe’ also contains a significant proportion of trust associated words. 

<p align="center">
  <img height=50% width=50% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/Vader_Sentiment.png">
</p>

VADER applies a heuristic process to sentiment analysis and is applied at the sentence-level. In addition to applying a lexicon set of features, VADER also incorporates polarity and intensity and uses heuristics to identify relevant punctuation, emoticons, and other features (such as all caps).

The subreddits with the highest degree of positive sentiment (as measured by ‘compound’) are: ‘flyfishing’, ‘Norway’, ‘camping’, ‘beatles’, and ‘gopro’. The subreddits ‘btc’, ‘soccer’, and ‘ProtectAndServe’ had the lowest, though still positive, compound sentiment values.

# Word Embedding
Word embeddings are a class of unsupervised algorithm for learning the meanings of words using word-context vector spaces. To derive the word context space, the word2vec algorithm was applied to a post-level bag of words. Subsequently t-SNE (t-distributed Stochastic Neighbor Embedding) was applied in order to cluster and visualize the high-dimensional vectors produced by word2vec. In the following t-SNE projections, words that show up in similar spaces tend to have similar contexts and meanings. 

While a word embedding matrix was calculated on the full corpus, more interesting visualizations were generated by calculating word embedding matrices on individual categories (which include two subreddits).

<p align="center">
  <img height=50% width=50% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/tSNE_hobby.png">
</p>

--- PICTURE tSNE_category_hobby.png ---

Several interesting word groups emerge from looking at the category ‘hobby,’ which includes the ‘camping’ and ‘flyfishing’ subreddits. Around (-5, 1) we find a group of words including ‘suggestions’, ‘advice’, ‘tips’, ‘recommendations’, and ‘help’. Around (7, 0.5) we find a group of words including ‘days’, ‘weekend’, ‘planning’, ‘trip’, ‘going’, ‘camping’, and ‘going’. These groups of words likely coincide with posts asking for information and planning excursions.

 # Similarity Measures
 
To calculate similarity, the document term matrix at the subreddit level was normalized and Cosine Similarity and Jensen Shannon Divergence were calculated. The associated dendrograms are presented below :

<p align="center">
  <img height=45% width=45% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/hca_JS.png">
</p>

<p align="center">
  <img height=45% width=45% src="https://github.com/johnleraas/Exploratory-Text-Analytics-Subreddits/blob/main/Docs/hca_cosine.png">
</p>

Interestingly, both distance measures cluster subreddits similarly to the categorizations chosen by the researchers. Jensen Shannon produces the same results, while Cosine Similarity calculates  ‘Norway’ (‘geo’ category)  closest to ‘flyfishing’ and ‘camping’ (‘hobby’ category) (though ‘flyfishing’ and ‘camping’ are more similar to each other). Additionally, Cosine Similarity calculates ‘france’ (‘geo’ category) closest to ‘rugbyunion’ and ‘soccer’ (‘sports’ category) (again, ‘rugbyunion’ and ‘soccer’ are more similar to each other). This categorization may be partially due to the popularity of outdoor activities in Norway and the choice of including primarily European sports (rugby and soccer) over American sports in the dataset. Notably, the height of the dendrograms suggests that the individual subreddits are fairly unique. 

The categories with the most similar subreddits seem to be ‘crypto’ (‘dogecoin’, ‘btc’) and ‘profession’ (‘ProtectAndServe’, ‘Firefighting’) based on both distance measures, though this is simply a reflection of the subreddit selections associated with the dataset.

# Conclusions

Though the 20 subreddits selected for this project may not be representative of the full 1,103 subreddit dataset, the subsampled dataset seems to possess subreddits that are relatively distinct. This is exemplified by the PCA results, particularly the relatively uniform portion of variance explained by the first 19 components. The dendrograms furthermore illustrate a significant amount of dissimilarity between different subreddits, given the distance between links. The plotting of various category or subreddit specific word embeddings also illustrates the unique language associated with various categorizations. 

Of the subreddits studied, ‘flyfishing’, ‘Norway’, ‘camping’, ‘beatles’, and ‘gopro’ possessed the highest sentiment as measured by VADER. Additionally ‘beatles’, ‘TaylorSwift’, and ‘FORTnITE’ exhibited the most joy, trust, and anticipation emotions based upon the NRC lexicon.
