---
layout: post
title: What Inception Net Doesn't See
comments: true
published: true
image: 
excerpt: Deep learning-based comuter vision models like Inception Net have achieved state-of-the-art performance on image recognition. However, that doesn't mean that they don't have blindspots and biases. Here's a few of them, along with interactive aplications for you to try it out yourself.
---

<style>
a[target="_blank"]::after {
  content: "";
  width: 1em;
  height: 1em;
  margin: 0 0.05em 0 0.1em;
  background: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxNiAxNiIgd2lkdGg9IjE2IiBoZWlnaHQ9IjE2Ij48cGF0aCBkPSJNOSAyTDkgMyAxMi4zIDMgNiA5LjMgNi43IDEwIDEzIDMuNyAxMyA3IDE0IDcgMTQgMlpNNCA0QzIuOSA0IDIgNC45IDIgNkwyIDEyQzIgMTMuMSAyLjkgMTQgNCAxNEwxMCAxNEMxMS4xIDE0IDEyIDEzLjEgMTIgMTJMMTIgNyAxMSA4IDExIDEyQzExIDEyLjYgMTAuNiAxMyAxMCAxM0w0IDEzQzMuNCAxMyAzIDEyLjYgMyAxMkwzIDZDMyA1LjQgMy40IDUgNCA1TDggNSA5IDRaIi8+PC9zdmc+) no-repeat;
  background-size: contain;
  display: inline-block;
  vertical-align: text-bottom;
}
</style>

Everyone knows that deep learning vision models like Inception Net achieve state-of-the-art performance on image recognition. However, I'm curious about when these models *don't work well*. I tested Inception Net on a large number of natural images, and here is a collection of things that the model doesn't predict well. Note: I'm only including **examples where the correct class is one of the 1,000 valid ImageNet classes**.

*Try it Yourself*: Along with each example is a link to the model demo (built with [www.gradio.app](www.gradio.app)) where you can try the image yourself directly from the browser.

### 1. Upside down cars

* Inception Net doesn't recognize the **car** category at all when the car is upside down

{% include image.html name="car1.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#1' target='_blank'>Explore this example in your browser</a>"%}


{% include image.html name="car2.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#2' target='_blank'>Explore this example in your browser</a>"%}

### 2. Sliced apples

* Although Inception Net recognizes many sliced fruits, it is convinced that sliced **apples** are actually  **cucumbers** or other fruits.

{% include image.html name="apple1.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#3' target='_blank'>Explore this example in your browser</a>"%}

{% include image.html name="apple2.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#4' target='_blank'>Explore this example in your browser</a>"%}

### 3. Pakistani grooms

* Inception Net can detect Western-dressed **grooms** quite well, but not when they're wearing Pakistani / Indian attire

{% include image.html name="groom1.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#5' target='_blank'>Explore this example in your browser</a>"%}

{% include image.html name="groom2.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#6' target='_blank'>Explore this example in your browser</a>"%}

### 4. Long balloons

* "If it isn't round, it isn't a **balloon**" -- Inception Net 

{% include image.html name="balloon1.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#7' target='_blank'>Explore this example in your browser</a>"%}

{% include image.html name="balloon2.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#8' target='_blank'>Explore this example in your browser</a>"%}

### 5. Regular surgical masks

* Admittedly, ImageNet was collected way before COVID-19, when these were less common. However, it's strange that even though **masks** and **gas masks** are valid categories, they are not predicted for regular surgical masks.

{% include image.html name="mask1.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#9' target='_blank'>Explore this example in your browser</a>"%}

{% include image.html name="mask2.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#10' target='_blank'>Explore this example in your browser</a>"%}


### 6. Cartoon lions

* Generally Inception Net fails at classifying cartoon versions of images. In my experience, cartoon **lions** were particularly hard for it to classify correctly.

{% include image.html name="lion1.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#11' target='_blank'>Explore this example in your browser</a>"%}

{% include image.html name="lion2.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#12' target='_blank'>Explore this example in your browser</a>"%}

### 7. Roombas

* It looks like Inception Net doesn't know about the Roomba **vacuum cleaner** as it consistently misidentifies it as another gadget instead

{% include image.html name="roomba1.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#13' target='_blank'>Explore this example in your browser</a>"%}

{% include image.html name="roomba2.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#14' target='_blank'>Explore this example in your browser</a>"%}


### 8. Geese in the Sky

* Inception Net wouldn't be good at threat detection as it mistakes these harmless **geese** in the sky for warplanes!

{% include image.html name="geese1.png" caption="<a href='https://github.com/hendrycks/natural-adv-examples' target='_blank'>Credit for this example goes to Hendrycks et al.</a> <a href='https://gradio.app/g/hub-inception-blindspots#15' target='_blank'>Explore it in your browser</a>"%}

{% include image.html name="geese2.png" caption="<a href='https://gradio.app/g/hub-inception-blindspots#16' target='_blank'>Explore this example in your browser</a>"%}

### Takeaways

So we've found some examples where Inception Net fails... so what? Well, deep learning-based image classifiers like Inception Net are believed to be highly accurate, to the point that many companies release them as APIs [1, 2, 3] designed to be used by the *general public*. The release is rarely accompanied by descriptions of *failure points* of the model, so users don't really know when to expect the model to be reliable and when it will not. 

By simply experimenting with the model via a web interface, I found several blind spots of Inception Net: for example, objects in unusual positions (#1) or items from non-Western cultures (#3) or even images in non-standard artistic styles (#6). These failure points should be thoroughly investigated and documented so that users known when to trust deployed image recognition models.

It was really easily to try Inception Net with images that were accessible for me (I simply did Google Image searches or tried images from my computer). Try the model yourself (http://www.gradio.app/g/inception) and let everyone know in the comments what other blindspots you find!

---

[1] https://cloud.google.com/vision

[2] https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/

[3] https://imagga.com/

