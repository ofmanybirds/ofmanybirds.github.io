---
layout: default
title: "Tag Usage Tracker"
---

# Tag Usage Tracker

Tags sorted by number of occurrences (highest first).

{% comment %}
Step 1: Flatten all tags into a single array
{% endcomment %}
{% assign all_tags = "" %}
{% for item in site.data.links %}
  {% if item.tags %}
    {% for tag in item.tags %}
      {% assign all_tags = all_tags | append: tag | append: "," %}
    {% endfor %}
  {% endif %}
{% endfor %}

{% assign tag_array = all_tags | split: "," | sort_natural %}

{% comment %}
Step 2: Count occurrences per tag using current/count trick
{% endcomment %}
{% assign current = "" %}
{% assign count = 0 %}
{% assign counts_list = "" %}

{% for tag in tag_array %}
  {% if tag != "" %}
    {% if tag != current %}
      {% if current != "" %}
        {% assign counts_list = counts_list | append: current | append: "|" | append: count | append: "," %}
      {% endif %}
      {% assign current = tag %}
      {% assign count = 1 %}
    {% else %}
      {% assign count = count | plus: 1 %}
    {% endif %}
  {% endif %}
{% endfor %}

{% if current != "" %}
  {% assign counts_list = counts_list | append: current | append: "|" | append: count | append: "," %}
{% endif %}

{% assign counts_array = counts_list | split: "," %}

{% comment %}
Step 3: Manually loop numeric counts descending (assume max 1000)
{% endcomment %}
<ul>
{% for c in (1000..1) %}
  {% for pair in counts_array %}
    {% if pair != "" %}
      {% assign parts = pair | split: "|" %}
      {% if parts[1] == c %}
        <li>{{ parts[0] }} ({{ parts[1] }})</li>
      {% endif %}
    {% endif %}
  {% endfor %}
{% endfor %}
</ul>
