---
layout: default
---

<div class="posts">
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
