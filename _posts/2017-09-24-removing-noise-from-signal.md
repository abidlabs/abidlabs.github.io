---
layout: post
title: "3 Ways to Remove Noise from Data/Signal"
comments: true
published: true
image: https://cdn.pixabay.com/photo/2014/07/31/23/38/headphones-407190_960_720.jpg
---


{% include image.html name="headphones.jpg" caption="Removing noise from data is an important first step in machine learning. What can data scientists learn from noise-canceling headphones?" width="88%"%}

As data scientists and researchers in machine learning, we usually don't think about how our data is collected. We focus on analysis, not measurement. While that abstraction is useful, it can be dangerous if we're dealing with noisy data. A dirty dataset can be a bottleneck that reduces the quality of the entire analysis pipeline.

In this post, I'm not going to talk about data collection. But I do want to talk about **what to do if you are given a dirty dataset.** Are there any steps you can easily take to improve the quality of your data? I will mention 3 high-level ideas to *denoise* data. As we'll see, each of these methods has an analogue in signal processing, as electrical engineers have been thinking about similar problems for a long time!

## 1. Get More Data

The first and simplest approach that you can do is to ask the person who gave you dirty data to give you *more of it*. Why does having more data help? Won't it also increase the amount of noise you have to deal with? 

To answer that question, let's go back to 1948 when **Claude Shannon discovered a quantitative relationship between communication capacity and the signal-to-noise ratio** of a communication channel -- an equation that started the entire field of information theory. We don't need the exact relationship here, but the proportionality is:

$$ C \propto \log_2{(1 + SNR)} $$ 

This equation is saying that the better your signal-to-noise ratio (\\(SNR\\)), the more information (\\(C\\)) you can communicate over the channel per unit time ([under certain assumptions](https://en.wikipedia.org/wiki/Shannon%E2%80%93Hartley_theorem)). Why are these two things (\\(C\\) and \\(SNR\\)) related? Because, if you have a noisy channel, you can compensate for it by adding **redudancy**: repeatedly sending your signal and have the person on the receiving end vote to see what your original signal was. Here's how that would work:

Suppose you wanted to communicate the digit \\(0\\) to your friend, but when you text him, there's a small chance the \\(0\\) gets "lost" and your friend sees a random digit instead: a \\(0\\) or \\(1\\). So you could just text him the same number 5 times. On the other end, he receives:

$${0, 0, 0, 1, 0}$$

One of the digits has been corrupted, but he can simply take the majority vote, which produces: \\(0\\). Now, there are more complicated schemes as we'll see next, but for our purposes, the interesting thing to note is that our basic voting scheme works **even if the noise dominates the signal**. In other words, even if the probability that a digit is lost is more than 50%, this still works, because whereas noise is random, the signal is consistently \\(0\\). So, in expectation, there will still be more \\(0\\)'s  than \\(1\\)'s.

This brings us back to machine learning: why does having more dirty data help? Because if the noise in the data is random (this isn't always the case; we'll relax this assumption in Point #3), having more data will cause the effects of noise to start canceling each other out, while the effect of the underlying true signal will add up.

## 2. Look at Your Data in a New Light (or "Basis")

What if collecting additional data is too expensive? It turns out that often times you can remove noise directly from your signal, especially if your signal has structure to it. This structure is often times not visible in the original basis \[Note: a basis (*pl.* bases) is the domain used to reveal the information in a signal. Examples of bases are: the time domain (because you can plot an audio signal as function of time) and the frequency domain (because you can plot the same audio signal as a function of frequency)\] that you received your data. But a transformation of your data into a new basis  can reveal the structure.

Perhaps the best-known example of this is the **application of the Fourier Transform to audio signals**. If you take a recording of someone speaking, it's going to have all sorts of background noise. This may not be clear from the raw signal. But if you break your signal [down by frequencies](https://en.wikipedia.org/wiki/Fourier_transform), you'll see that most of the audio signal lies in just a few frequencies. The noise, because it's random, will still be spread out across all of the frequencies. This means that we can *filter* out most of the noise by retaining only the frequencies with lots of signal and removing all other frequencies.

What's the connection with cleaning up data? For this part, it helps to have a  specific dataset in mind: so consider a dataset that consists of \\(m\\) users who have each read \\(n\\) books and assigned each one a real-number rating (similar to the [Netflix Challenge](https://en.wikipedia.org/wiki/Netflix_Prize)). This dataset can be succinctly described by a matrix \\(A \in R^{m \times n}\\). Now, suppose this data matrix is heavily corrupted (i.e. by Gaussian noise or by missing entries).

Can we clean this dataset up by finding a better basis to project this data? Yes, a very common way is to use **singular value decomposition** (SVD). SVD allows us to write rewrite a matrix in terms of a new basis:

$$ A = \sum_{i=1}^{k} s_i u_i v^T_i $$

Here, we can think of each \\(u_i v_i^T\\) as a _basis vector_ (which can be thought of as representing some particular characteristic of the books that affects users' ratings, e.g. the 'amount of suspense' or 'whether the characters are animals'), and each \\(s_i\\) (singular value) reflects the contribution of that basis in the decomposition of \\(A\\) (so \\(s_i\\) would represent the relative importance of 'suspense' to 'character species' in the final rating). Here, \\(k\\) represents the [rank](https://en.wikipedia.org/wiki/Rank_(linear_algebra)) of the matrix. However, even if the matrix is full-rank, usually only the first few values of \\(s_i\\) are large. What this means is that we can ignore all of the basis vectors with small singular values without significantly changing \\(A\\). (this is the reason [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis) works!)

This is really nice, because if the noise is random, it will still be spread out across all of the basis vectors. That means we can discard all of the basis vectors with low singular values, leaving us with a few basis vectors that the signal is concentrated in, diluting the effect of the noise. [Many teams that did well on the Netflix Challenge](http://sifter.org/simon/journal/20061211.html) used this idea in their algorithms.

# 3. Use a Contrastive Dataset

So far, we have assumed that the noise is random, or at the very least, that it does not have any significant structure in the new basis we have used to transform our data. But what if that's not the case? Imagine that you are trying to record audio signal from someone speaking, but there is a jet engine rumbling in the background. 

If you were to take the Fourier Transform of the audio signal and only look at the frequencies with the largest amplitude of sound, then **you would pick out the frequencies corresponding to the jet engine instead of the frequencies corresponding to human speech** if former are louder than the latter. In this case, it's probably more appropriate to think of the rumbling jet engine as a _confounding signal_ rather than noise, but nevertheless, fundamentally, it represents unwanted signal.

To combat this problem (and make things like noise-canceling headphones possible), electrical engineers have developed _adaptive noise cancellation_, a strategy that uses two signals: the _target_ signal, which is the corrupted sound, and a _background_ signal that only contains the noise. The background is essentially subtracted from the target signal to estimate the uncorrupted sound.

The same kind of approach can be used to clean dirty datasets that contain significant background trends that are not of interest to the researcher. Imagine, for example, that we have genetic measurements from cancer patients of different ethnicities and sexes. If we directly apply SVD (or PCA) to this data, the top basis vectors will correspond to the demographic variations of the individuals instead of the subtypes of cancer because the genetic variations due to the former are likely to be larger than and overshadow the latter. But if we have a background dataset consisting of healthy patients that also contains the variation associated with demographic differences, but not the variation corresponding to subtypes of cancer, we can **search for the top basis vectors in our primary dataset that our absent from the background**.

Some colleagues and I have developed a technique to do this analysis in a principled way. The technique is called **Contrastive PCA**. Check out our paper for more details and experiments: https://arxiv.org/abs/1709.06716

<br>&nbsp;
<br>&nbsp;

--

This post was cross-listed at my [lab's website](https://zou-group.github.io/article/removing-noise-from-signal/).
