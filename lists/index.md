---
layout: default
title: Lists
---
<h1>Lists</h1>

<ul>
{% assign list_pages = site.pages
   | where: "section", "lists"
   | where: "list", true
   | sort: "title" %}

{% for p in list_pages %}
  <li>
    <a href="{{ p.url }}">{{ p.list_title | default: p.title }}</a>
    {% if p.order %} (Order: {{ p.order }}){% endif %}
  </li>
{% endfor %}
</ul>
