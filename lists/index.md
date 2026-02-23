---
layout: default
---

<h1>Lists</h1>

<ul>
{% assign list_pages = site.pages | where_exp: "p", "p.path contains 'lists/' and p.name != 'index.md'" %}
{% assign sorted = list_pages | sort: "title" %}

{% for p in sorted %}
  <li>
    <a href="{{ p.url }}">{{ p.title }}</a>
  </li>
{% endfor %}
</ul>
