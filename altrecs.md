---
layout: default
---

<p>A non-specific list of things we happen to find interesting. These are generally not endorsements, and the exceptions are tagged as such.</p>

{% assign grouped = site.data.links | group_by: "category" | sort: "name" %}
{% for group in grouped %}
  <section>
    <h2>{{ group.name | capitalize }}</h2>
    <ul>
    {% for item in group.items %}
      <li>
        <a href="{{ item.url }}">{{ item.title }}</a>
<small>
  [
  {% if item.tags and item.tags.size > 0 %}
    {% for tag in item.tags %}{{ tag }}{% unless forloop.last %}, {% endunless %}{% endfor %}
  {% else %}
    untagged
  {% endif %}
  ]
</small>
      </li>
    {% endfor %}
    </ul>
  </section>
{% endfor %}
