---
layout: default
title: "Tag Usage Tracker"
---

# Tag Usage Tracker

This page lists all tags used in the site, sorted by number of occurrences.

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
Step 3: Build a unique list of counts, sorted descending numerically
{% endcomment %}
{% assign count_values = "" %}
{% for pair in counts_array %}
  {% if pair != "" %}
    {% assign parts = pair | split: "|" %}
    {% assign count_values = count_values | append: parts[1] | append: "," %}
  {% endif %}
{% endfor %}

{% assign unique_counts = count_values | split: "," | uniq | sort_natural | reverse %}

<ul>
{% comment %}
Step 4: Loop counts descending and print tags with that count sorted alphabetically
{% endcomment %}
{% for c in unique_counts %}
  {% assign tags_with_count = "" %}
  {% for pair in counts_array %}
    {% if pair != "" %}
      {% assign parts = pair | split: "|" %}
      {% if parts[1] == c %}
        {% assign tags_with_count = tags_with_count | append: parts[0] | append: "," %}
      {% endif %}
    {% endif %}
  {% endfor %}
  {% assign tags_list = tags_with_count | split: "," | sort_natural %}
  {% for tag in tags_list %}
    {% if tag != "" %}
      <li>{{ tag }} ({{ c }})</li>
    {% endif %}
  {% endfor %}
{% endfor %}
</ul>
