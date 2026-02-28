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
        {% comment %} pad count to 4 digits {% endcomment %}
        {% assign padded = count | prepend: "0000" | slice: -4, 4 %}
        {% assign counts_list = counts_list | append: padded | append: "|" | append: current | append: "," %}
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
  {% assign counts_list = counts_list | append: padded | append: "|" | append: current | append: "," %}
{% endif %}

{% assign counts_array = counts_list | split: "," | sort_natural | reverse %}

<ul>
{% for pair in counts_array %}
  {% if pair != "" %}
    {% assign parts = pair | split: "|" %}
    {% assign display_count = parts[0] | remove_first: "0" | remove_first: "0" | remove_first: "0" | remove_first: "0" %}
    {% if display_count == "" %}{% assign display_count = "0" %}{% endif %}
    <li>{{ parts[1] }} ({{ display_count }})</li>
  {% endif %}
{% endfor %}
</ul>
