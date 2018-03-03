---
layout: default
---

<div class="posts">

    <article class="post">

      <div class="entry">
      &#9733; On the advice of my professor, <a href="https://theorydish.blog/2018/02/26/donald-knuth-on-writing-up-research/">as well as David Knuth</a>, I'm going to start a <strong>research journal</strong>, where I communicate theoretical and conceptual ideas that I am exploring. 

      I appreciate feedback! Just click on the title of a journal entry to leave a comment.      
      </div>

      
    </article>

  {% for post in site.posts %}
    {% if post.categories contains 'journal' %}
    
    <article class="post">

      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.date | date: "%e %B %Y" }}</a></h1>

      <div class="entry">
        {{ post.content }}
      </div>

      
    </article>
  {% endif %}
  {% endfor %}
</div>
