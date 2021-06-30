---
layout: post
title: I “Coded” a Web App to Translate and Summarize the News
comments: true
published: true
image: https://lebanesestudies.news.chass.ncsu.edu/wp-content/uploads/sites/11/2018/01/Newspapers_image2.jpg
excerpt: Every now and then, I’m working on a project, when I’m amazed by the sheer power of reusable code. This happened with my last project, where I became interested in how the Arab media reports news compared to its American counterparts.
---


Every now and then, I’m working on a project, when I’m amazed by the sheer power of reusable code. This happened with my last project, where I became interested in **how the Arab media reports news compared to its American counterparts**. As I sketched out my project idea, I envisioned 3 pieces:

1. Scrape an Arab news website (e.g. Al Jazeera) to find articles on a given topic (e.g. “Afghanistan”)
1. Translate them to English
1. Summarize them and display them in a simple web app

<img src="https://i.ibb.co/vYcsxgr/steps.png" width="80%">

As I started implementing this project, I did some research on what libraries are out there for similar tasks. I quickly realized that I wouldn’t need to write much code at all. Each of these 3 pieces could be abstracted away by an awesome Python library: `newspaper`, `transformers`, and `gradio` respectively.

And indeed, the full app ([available in this colab notebook](https://colab.research.google.com/drive/12ovJmWzA1USQMMh5dKTbHdRh0V9-QsQC#scrollTo=vTKc8nGY-wyt)) is just 15 lines of Python code, and you can see it in action here:

<img src="https://s6.gifyu.com/images/recording-37.gif" width="80%">

Since there’s so little coding to do, what I’d like to do instead is to dive into the three libraries I used and why I think they are incredible libraries. 

[Newspaper3k (11k stars)](https://github.com/codelucas/newspaper): scrape almost any news website

The first thing that I needed to do was to scrape Arab news websites like Al Jazeera. I was intending on scraping the text using general-purpose web scraping libraries like `beautifulsoup` but that would still have required me to write code to filter the information from the correct HTML tags. To my delight, I found that the `newspaper` library already does this for all major news websites across many different languages. With just a few lines of code, I was able to get the text (or authors, publish time, or other metadata) of any article on Al Jazeera. Even better, the library was automatically able to serve up the text from the latest articles on Al Jazeera for my perusal.

[HuggingFace Transformers (47k)](https://github.com/huggingface/transformers): use state-of-the-art natural language models

A few years ago, many “model zoos” popped up to showcase the latest and greatest machine learning models (e.g. PyTorch Hub, TensorFlow Hub). Despite the apparent usefulness of these model zoos and the ease with which they made it easy to use these models, they never really took off among ML practitioners. For example, PyTorch Hub really only has 50 models, and I can’t remember the last time I used one of their models besides some of the standard image classifiers. Part of it is that the models are usually “not exactly” what I need -- for example, I need something to classify images of 3 different kinds of furniture, whereas the model on PyTorch Hub is a general-purpose image classifier.

HuggingFace’s model zoo feels different. First of all is the sheer number of models. I remember seeing the HF model zoo a few months ago, when I think it had a few hundred models, and I remember being impressed by the number. I checked again a few days ago, and to my shock, it has more than 10,000 models! 

It has models fine-tuned for almost any conceivable task. I was able to find *multiple* models for both translating from Arabic to English, as well as summarizing news articles. These models are available through the `transformers` library, or through web-based APIs, which is ultimately what I ended up using (more on that below).

[Gradio (3k)](https://gradio.app/): build interactive web-based demos

The final piece of my web application was to build a little demo that would allow me to interactively try different topics and see what kind of news summaries would show up. Rather than building a web application with HTML, CSS, and React, I used `gradio`, which is a library I helped build myself. The library makes it easy to build demos for machine learning models. 

I also took advantage of a neat integration between `gradio` and HuggingFace, which allows you to call HuggingFace’s web-based APIs and create a demo with a single line of Python code. In fact, you’ll see that we don’t even import the `transformers` in the [colab notebook](https://colab.research.google.com/drive/12ovJmWzA1USQMMh5dKTbHdRh0V9-QsQC#scrollTo=vTKc8nGY-wyt) directly, we just use them through `gradio`.

--------

Software abstractions, when well-designed, are powerful because they allow us to use existing code in modular ways. With the libraries above, one can imagine using machine learning models as lego pieces to analyze various things about the news in all sorts of different languages, and displaying them to users in interactive ways.
