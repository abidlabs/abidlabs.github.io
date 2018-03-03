---
layout: default
---

<div class="posts">

    <article class="post">

      <div class="entry">
      On the advice of my professor, [as well as David Knuth](https://theorydish.blog/2018/02/26/donald-knuth-on-writing-up-research/), I'm going to start a **research journal**, where I communicate theoretical and conceptual ideas that I am exploring. 

      I appreciate feedback! Just click on the title of a journal entry to leave a comment.      
      </div>

      
    </article>

  {% for post in site.posts %}
    {% if post.categories contains 'journal' %}
    
    <article class="post">

      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>

      <div class="entry">
        {{ post.content }}
      </div>

      
    </article>
  {% endif %}
  {% endfor %}
</div>
