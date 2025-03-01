---
layout: default
title: "Notes"
permalink: /notes/
---

<h1>Notes</h1>

{% for note in site.collections.notes.docs %}
  <h2><a href="{{ note.url }}">{{ note.title }}</a></h2>
  <p>{{ note.date | date: "%B %d, %Y" }}</p>
  <p>{{ note.excerpt }}</p>
  <hr>
{% endfor %}
