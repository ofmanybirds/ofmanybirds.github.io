---
layout: default
title: "Tag Usage Tracker"
---

# Tag Usage Tracker

Tags sorted by count descending (ties arbitrary).

{% assign all_tags = "" %}
{% for item in site.data.links %}
  {% if item.tags %}
    {% for tag in item.tags %}
      {% assign all_tags = all_tags | append: tag | append: "," %}
    {% endfor %}
  {% endif %}
{% endfor %}

{% assign tag_array = all_tags | split: "," | sort_natural %}

{% assign current = "" %}
{% assign count = 0 %}
{% assign padded_array = "" %}

{% for tag in tag_array %}
  {% if tag != "" %}
    {% if tag != current %}
      {% if current != "" %}
        {% assign padded = count | prepend: "0000" | slice: -4, 4 %}
        {% assign padded_array = padded_array | append: padded | append: current | append: "," %}
      {% endif %}
      {% assign current = tag %}
      {% assign count = 1 %}
    {% else %}
      {% assign count = count | plus: 1 %}
    {% endif %}
  {% endif %}
{% endfor %}

{% if current != "" %}
  {% assign padded = count | prepend: "0000" | slice: -4, 4 %}
  {% assign padded_array = padded_array | append: padded | append: current | append: "," %}
{% endif %}

{% assign sorted_array = padded_array | split: "," | sort_natural | reverse %}

<ul>
{% for item in sorted_array %}
  {% if item != "" %}
    {% assign display_count = item | slice: 0,4 | remove_first: "0" | remove_first: "0" | remove_first: "0" | remove_first: "0" %}
    {% if display_count == "" %}{% assign display_count = "0" %}{% endif %}
    {% assign tag_name = item | slice: 4, 100 %}
    <li>{{ tag_name }} ({{ display_count }})</li>
  {% endif %}
{% endfor %}
</ul>
