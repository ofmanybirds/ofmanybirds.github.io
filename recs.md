---
layout: default
title: "Recommendations"
---

<h1>{{ page.title }}</h1>
<p>A curated repository of links, sortable by category and tag.</p>

{% assign grouped = site.data.links | group_by: "category" | sort: "name" %}
{% for group in grouped %}
  <section>
    <h2>{{ group.name | capitalize }}</h2>
    <ul>
    {% for item in group.items %}
      <li>
        <a href="{{ item.url }}">{{ item.title }}</a>
        {% if item.tags %}
          <small>
            [{% for tag in item.tags %}{{ tag }}{% unless forloop.last %}, {% endunless %}{% endfor %}]
          </small>
        {% endif %}
      </li>
    {% endfor %}
    </ul>
  </section>
{% endfor %}
