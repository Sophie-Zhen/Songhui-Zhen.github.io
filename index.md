---
layout: home
---

# Welcome to My NLP Learning Journey

{% for post in site.posts %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  <p>{{ post.date | date: "%B %d, %Y" }}</p>
  <p>{{ post.excerpt }}</p>
  <hr>
{% endfor %}

{% for note in site.notes %}
  <h2><a href="{{ note.url }}">{{ note.title }}</a></h2>
  <p>{{ note.date | date: "%B %d, %Y" }}</p>
  <p>{{ note.excerpt }}</p>
  <hr>
{% endfor %}