---
layout: post
title: Can a Machine Learn to Classify Meccan and Madinan Surahs?
published: true
---

One basic concept in Quranic studies is the difference between its **meccan** chapters (revealed before the Prophet migrated from trading city of Mecca to oasis town of Medina) and its **medinan** chapters (revealed after the migration). 

Knowing where a chapter was revelead helps scholars derive meaning from the text, and so a great deal of effort has been expended to finding narrations that mention when a particular verse or chapter was revealed. However, the location where a chapter was revealed is also manifested in the linguistic characteristics of the passage. For example, meccan chapters are characterized by poetic meter, a rhetorical urgency, and an emphasis on recognizing the Divine, while medinan chapters are more lengthy prose dedicated to explaining religious ritual. 

This made me think that it might be possible to build a binary classifier that could use basic techniques from natural language processing to classify surahs as medinan or meccan. Here, I outline my methodology and share my results.

#Methodology



- First download all of the verses of the Quran (without harakaat), along with surah number and ayah number
- Then download whether they are meccan or medinan
- Split the surah into 2 sets, training or test
- For each verse in each surah, look at the Arabic word, and make a vector of words representation (may require conversion to transliterated Arabic)

- Create a matrix of all the verse-vectors, along with a vector of the labels
- Build a linear regression system
- Do performance on test set
- Report accuracy on verses, as well as on "ensembles"
- Show top "Meccan" words and top "Madinan" words

- Conclusion
This could be a tool to help scholars, those verses that are not totally clear
