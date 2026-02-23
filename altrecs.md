---
layout: default
---

<p>A non-specific list of things we happen to find interesting. These are generally not endorsements, and the exceptions are tagged as such.</p>

{% comment %}
  Step 1: sort categories naturally
{% endcomment %}
{% assign grouped = site.data.links | group_by: "category" | sort_natural: "name" %}

{% for group in grouped %}
  <section>
    <h2>{{ group.name | capitalize }}</h2>
    <ul>
      {% comment %}
        Step 2: sort items naturally by title
      {% endcomment %}
      {% assign sorted_items = group.items | sort_natural: "title" %}
      {% for item in sorted_items %}
        <li>
          <a href="{{ item.url }}">{{ item.title }}</a>
          {% if item.tags and item.tags.size > 0 %}
            <small>[
              {% for tag in item.tags %}{{tag}}{% unless forloop.last %}, {% endunless %}{% endfor %}
            ]</small>
          {% else %}
            <small>[untagged]</small>
          {% endif %}
        </li>
      {% endfor %}
    </ul>
  </section>
{% endfor %}
