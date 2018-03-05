---
layout: post
title: "Moral Machine Learning"
comments: true
published: true
image: https://upload.wikimedia.org/wikipedia/commons/1/16/Social_Determinants_of_Health_Infoviz.jpg
---

Is machine learning research making the world a better place? I think about this sometimes when I attend conferences and see poster after poster proving theoretical guarantees for machine learning algorithms that already work well, or algorithm after algorithm that claims to achieve records results on standard image recognition datasets. 

Certainly, there are benefits of image recognition technology to those people who are visually impaired, for example, but it seems that the biggest gains of machine learning research goes to increase the comfort of those already quite comfortable. And while undoubtedly a wide variety of devices and apps have integrated basic machine learning techniques as part of their core technology, it may seem that the cutting-edge research that is being carried out in the best universities isn't really addressing fundamental problems that are faced by humanity. Can machine learning offer some sort of solutions to these problems?

For this post, I searched for researchers that are really trying to put machine learning to good use, to solving what I believe are critical problems faced by vulnerable people. For anyone who believes in an ethical thrust to their research, I hope these problems and datasets can provide some inspiration or direction.

# 1. Predicting Poverty Using Satellite Imagery

In the realm of computational sustainability is [research from Stefano Ermon's Lab](http://sustain.stanford.edu/predicting-poverty/) at Stanford, which combines satellite images to predict what parts of a country are suffering the highest rates of poverty. This is important because in many parts of the developing world, it is difficult to carry out surveys to estimate consumption expenditure and allocate aid accordingly.

# 2. Helping Doctors Identify Rare Diseases 

Doctors are great at diagnosing common diseases, and even though machine learning is making some inroads in radiology and cancer dosing, it seems unlikely that doctors will be replaced by machines in the near future. But there is one area where doctors really can use some help: rare diseases that doctors may not have frequently encountered during their training or practice can be aggregated into a database. If a doctor is stuck, he or she can just put in patient symptoms, and [get disease predictions as outputs](https://arxiv.org/ftp/arxiv/papers/1609/1609.01586.pdf).

# 3. Understanding How to Improve the Wellbeing of Children

The Fragile Families and Child Wellbeing Study [hosted a competition this year](https://www.princeton.edu/news/2017/11/13/fragile-families-challenge-uses-big-data-answer-big-questions), in which they provided machine learning researchers with the wellbeing data from 5,000 children: their GPA, material hardship, employment of caregivers, and so on, in an effort to understand what factors are critical for childcare success. The winning team, from the MIT Media Lab, created a model that can be used by social scientists to conduct more focused methodological research.

# 4. Scaling Education using Virtual Teaching Assistant

Education has always [historically very tricky to automate](http://teachingmachin.es/timeline.html), and the general consensus is that despite the massive interest and investment in open course-ware in the last few years, they have not quite lived up to their performance. It seems that humans just learn best from other humans. But what if they can't tell? An [intriguing experiment](http://www.news.gatech.edu/2016/05/09/artificial-intelligence-course-creates-ai-teaching-assistant) by a machine learning professor at Georgia Tech showed that a virtual teaching assistant could answer many students' questions, and the students didn't even realize that "she" was not a human! This might hold the key to scale eduction to communities where teacher time and teaching resources are limited.

# 5. Detecting Epidemics from News Articles

It takes a lot of resources to detect an epidemic like Ebola or swine flu: you need to continuously monitor every location around the world for unusual symptoms reported by lots of patients. Machine learning can lend a hand, as has been shown by Harvard researchers who [built HealthMap](http://www.healthmap.org/en/) to aggregate news articles reporting any sort of unusual symptoms in order to detect outbreaks. Before the World Health Organization recognized the 2014 Ebola epidemic, they were able to detect many hospitals reporting "mystery hemorrhagic fever" in Guinea.

# 6. Reducing Bias in the Courtroom

There has been significant discussion among researchers about whether machine learning, which is currently deployed in many courtrooms to determine whether defendants should get bail, reduce or aggravate racial biases. This has sparked a massive interest in ensuring [fairness in algorithms](https://icme.stanford.edu/sites/default/files/algo-fairness.pdf), or producing machine learning methods that treat certain sensitive attributes equally. Even if current algorithms do not meet these standards, the fact that we can quantify performance along fairness metrics means that we can potentially develop algorithms that are less biased than human judges.

# 7. Understanding Disease Trajectories of Understudied Populations

I'll end with an example from our lab here at Stanford. We have access to the [UK Biobank](http://www.ukbiobank.ac.uk/), a massive dataset of 500,000 patients from the UK from a variety of demographic backgrounds. This gives us an opportunity to carry out _contrastive_ studies, where we can understand what diseases and disease patterns are over-expressed in women and minority groups. This generates important knowledge because much of clinical knowledge is derived from studies that are done on middle-aged, Caucasian, men.