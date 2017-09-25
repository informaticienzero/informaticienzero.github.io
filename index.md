---
layout: default
title: Index
permalink: /
---

Vous voici à l'index de ce superbe site !

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
