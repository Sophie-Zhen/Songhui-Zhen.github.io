---
layout: default
title: "Notes"
permalink: /notes/
---

<h1>Notes</h1>

{% for post in site.categories.notes %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  <p>{{ post.date | date: "%B %d, %Y" }}</p>
  <p>{{ post.excerpt }}</p>
  <hr>
{% endfor %}
