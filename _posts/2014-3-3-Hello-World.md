---
layout: post
title: Can a Machine Learn to Classify Meccan and Medinan Surahs?
published: true
image: https://upload.wikimedia.org/wikipedia/commons/e/e2/Kabaa.jpg
---

A basic concept in Quranic studies is the difference between its **meccan** surahs/chapters (revealed before the Prophet migrated from trading city of Mecca to oasis town of Medina) and its **medinan** chapters (revealed after the migration). 

Knowing where a chapter was revealed helps scholars derive meaning from the text, and so a great deal of effort has been expended to finding narrations that mention when a particular verse or chapter was revealed. However, the location where a chapter was revealed is also manifested in the linguistic characteristics of the passage. For example, meccan chapters are characterized by poetic meter, a rhetorical urgency, and an emphasis on recognizing the Oneness of God, while medinan chapters are more lengthy prose dedicated to explaining religious rituals.

This made me think that it might be possible to build a binary classifier that could use basic techniques from natural language processing to classify chapters as medinan or meccan. Here, I outline my methodology and share my results.

# Methodology

**Downloading the Dataset.** The first step is to download all of the text of the Quran. The standard text of the Quran can be downloaded from:

<http://tanzil.net/docs/download>

The version without vowel marks and with verse numbers will work best for our work. We also need to know which surahs are meccan and which are medinan. Find this information at:

<http://tanzil.net/docs/revelation_order>

Alternatively, a link to a github repository with the csv files (and all source code) is included at the end of this post.

**Word Vectors** Once we have the text of the Quran, we need to convert the text to a standardized representation that can be fed to machine learning algorithms. Usually, these representations are in the form of numeric arrays or tensors.

There are a variety of ways to do this, but the simplest may be the [bag-of-words technique](https://en.wikipedia.org/wiki/Bag-of-words_model), which represents a 'text' (in this case, we'll use each verse) as a vector, with each dimension representing the occurrence or frequency of a particular word.

As an example, let's say the words in our body of text are:

إنا -- الإنسان -- لفي -- خسر -- أعطينا -- الكوثر

Then, we could represent these two verses as follows:

| Verse         | Bag of Words Vector   |
| ------------- |:---------------------:| 
| إنا الانسن لفى خسر  | [1, 1, 1, 1, 0, 0] |
| إنا اعطينك الكوثر     | [1, 0, 0, 0, 1, 1] |

If a word repeats more than once, the vector reflects that multiplicity, so a vector can have entries more than 1 as well.

**Training and Validation**

Before feeding the word vectors into a classifier, we need to partition our dataset into a training and validation set. To ensure that the validation set and training set are independent, the partitioning is done on a *chapter level*, not a verse level. This is because some chapter include repeating refrains (e.g. Surah Al-Rahman), it is unfair to include those verses in both the validation and training sets. 

The partitioning was: Training: 40%, Validation: 60% (I chose a larger validation set to increase the meaningfulness of results, as will be discussed later). Because the partitioning was done on a chapter level, the number of verses in the training and validation set was not exactly 40/60.

**Logistic Regression**

The classifier I ended up choosing was logistic regression. Logistic regression works similarly to linear regression, as it assigns linear weights to the presence of each feature in our vector. But then it introduces a non-linearity -- by passing the result through the logistic function

$$ p(x) = \log{\frac{x}{1-x}} $$

This provides a probability between 0 and 1 that verse belongs to a particular class -- in our case, the classes are meccan or medinan. We train the weights on our training set, and check how well it performs on the test set.

# Results

So how did we do? The following table represents the training and validation accuracies on a typical run:

| Round         | Accuracy   |
| ------------- |:---------------------:| 
| Training     |  98.68% |
| Validation     | 86.26% | 

The training accuracy is very high because we are over-fitting on our datasets. This not surprising because the number of parameters in our model -- or the number of unique words in the Quran -- is 14,870, far more than the number of verses in our training set.

However, we do pretty decently on our validation set as well. The accuracy, 86%, needs to be taken within context:

* The dataset is unbalanced -- there are many more meccan chapters than medinan chapters, so if our classifier was just classifying every verse as meccan, it would get an accuracy of about 74%. Depending on the specific partitioning of the training and validation, we have an accuracy around 10 percentage points higher than that.
* This is on a verse-by-verse level, under the assumption that every verse in a meccan surah is meccan, and same for medinan. This assumption is definitely not true, which affects both our training and evaluation of performance.

A better indicator would be performance on a surah level. If we use a simply majority voting system (using all the verses in a chapter to "vote" for whether the surah is meccan or medinan), we get the following surah:

| Round         | Accuracy   |
| ------------- |:---------------------:| 
| Overall surah-level     |  94.73% |

I'll confess, I was a bit disappointed when I saw that this wasn't closer to 100%. Which surahs are misclassified? It turns out that there are 6 surahs that are misclassified:

| Misclassified Surah         | Predicted type   | Confidence (votes) |
| ------------- |:---------------------:|:---------------------------:| 
| Surah Ra'd (13)     |  meccan |  81% |     
| Surah Hajj (22)     |  meccan |  63% |
| Surah Muhammad (47)   |  meccan | 53% |
| Surah Rahman (55)      |  meccan | 100% |
| Surah Insaan (76)      |  meccan | 97% |
| Surah Zilzaal (99)    |  meccan | 100% |

When I took a closer look at these chapters, I was amazed to see that there is actually [scholarly disagreement](http://learnqurankareem.blogspot.com/2013/04/list-of-all-surah.html) about all 6 of these chapters! Based solely on word usage, our classifier provides that a few of these, such as Surahs Rahman, Insaan, and Zilzaal might fall in the meccan camp! 

Finally, I was curious to see which Arabic words are the "most meccan" and the "most medinan." We can analyze this by looking at the weights that the model learns for each word. The top 10 meccan and medinan words are:

|Top Meccan Words|Top Medinan Words|
|:-------------:|:--------------:|
| بعهدكم   | تقتلني|   
| وتقسطوا      | والله|   
| العالمون     | أكلها|   
| أولاهما      | وإذ|   
| لتحصنكم      | وبكفرهم|   
| العمى    | مبصرة|   
| نور      | زلزلة|   
| لمستقر   | بآية|   
| لنفد     | سخرناها|   
| تنزيل    | وجهرا|   


Perhaps those who have studied the Quran more can enlighten me, but these words don't seem to follow any significant trends. 

I did notice that the words in the meccan column are mostly those words that are *rare*, but make an appearance in at least once meccan surah. For example, لنفد is found only in Surah Al-Kahf. The words in the medinan column are sometimes also [hapaxes](https://en.wikipedia.org/wiki/Hapax_legomenon), but in other cases, *common* words in the Quran, which are overweighted in the medinan verses, which tend to be longer than meccan verses on average. So while *والله* would appear in both kinds of surahs, it might appear multiple times in a medinan surahs. A further confounding factor is that our simple "bag of words" technique is unable to recognize any degree of similarity between related words -- *والله* is a separate word than *الله* internally -- making it more difficult to build complex insights.

With a deeper look into the weights, one might be able to identify trends as to why certain words are more "meccan" and why others are more "medinan," but the most strongly weighted words seem to result of technicalities associated with bag-of-word techniques. (Perhaps *normalizing* vectors before feeding them into logistic regression would lead to a more interpretable model?).

# Conclusion

The basic conclusion is that we can design an extremely simple algorithm to classify meccan and medinan surahs, based on a small set of training examples! While its not clear the internal model that the algorithm builds is particularly sophisticated or enlightening, we see that it performs quite well -- only incorrectly classifying 6 surahs, all of which are the subject of scholarly debate anyway!

<hr>

*All the source code and data files for this project can be found and cloned from this <a href="https://github.com/abidlabs/classify-surahs">github repository</a>.*
