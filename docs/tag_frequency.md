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
Step 2: Count occurrences per tag
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
Step 3: Find max count
{% endcomment %}
{% assign max_count = 0 %}
{% for pair in counts_array %}
  {% if pair != "" %}
    {% assign parts = pair | split: "|" %}
    {% assign val = parts[1] | plus: 0 %}
    {% if val > max_count %}
      {% assign max_count = val %}
    {% endif %}
  {% endif %}
{% endfor %}

{% comment %}
Step 4: Build descending array from max → 1
Liquid has no numeric range with step, so we use a trick
{% endcomment %}
{% assign descending = "" %}
{% for i in (1..max_count) %}
  {% assign descending = descending | prepend: i | append: "," %}
{% endfor %}
{% assign descending_array = descending | split: "," %}

<ul>
{% for c in descending_array %}
  {% if c != "" %}
    {% for pair in counts_array %}
      {% if pair != "" %}
        {% assign parts = pair | split: "|" %}
        {% if parts[1] == c %}
          <li>{{ parts[0] }} ({{ parts[1] }})</li>
        {% endif %}
      {% endif %}
    {% endfor %}
  {% endif %}
{% endfor %}
</ul>
