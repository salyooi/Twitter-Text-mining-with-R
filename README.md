# Text mining using TwitteR, tm and wordcloud R packages:
# New path/steps to search TW using R:


consumer_key <- 'enter TW API code here'
consumer_secret <- 'enter TW API code here'
access_token <- 'enter TW API code here'
access_secret <- 'enter TW API code here'
setup_twitter_oauth(consumer_key, consumer_secret, access_token,access_secret)
# Install packages
install.packages("twitteR")
install.packages("wordcloud")
install.packages("tm")
library("twitteR")
library("wordcloud")
library("tm”)
# start pulling tweets from news agency
Tasnimnews_EN_tweets <- searchTwitter("Tasnimnews_EN", n=1000, lang = "en”)
# Test
Tasnimnews_EN_tweets
Tasnimnews_EN_tweets[1:10]
# To search TW for one or two more keywords
ib <- searchTwitter('iran+brazil', lang="en", n=1000, resultType = "recent”)
# test
ib
# start cleaning up data
ib_text <- sapply(ib, function(x) x$getText())
# create corpus from vector of tweets
irb_corpus <- Corpus(VectorSource(ib_text))
# test
irb_corpus
# you should get something looks like this
"Metadata:  corpus specific: 1, document level (indexed): 0
Content:  documents: 50”
# to look the first document in the corpus
inspect(irb_corpus[1])
# start clean up the text
irb_clean <- tm_map(irb_corpus, removePunctuation)
irb_clean <- tm_map(irb_corpus, content_transformer(tolower))
irb_clean <- tm_map(irb_corpus, removeWords, stopwords("english"))
irb_clean <- tm_map(irb_corpus, removeNumbers)
irb_clean <- tm_map(irb_corpus, stripWhitespace)
irb_clean <- tm_map(irb_corpus, removeWords, c("iran", "brazil"))
# for last function, you can remover leave main keywords/search words (Iran and brazil) from wordcloud
# test
wordcloud(irb_clean)
#  you should get black and white wordcloud, not 100% clean
# next, play with scale, organization and color for better aesthetic look
# I don’t have the explanation now, but if you add scale and maximum words to the function will give you wordcloud with much colors. Bolow the two functions to try
# option 1
wordcloud(irb_clean, random.order = F, max.words = 40, scale = c(3, 0.5), colors = rainbow(50))
# option 2
wordcloud(irb_clean, random.order = F, colors = rainbow(50))
