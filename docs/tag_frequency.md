---
layout: default
---

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
  Step 2: Count occurrences per tag using the same "current/count" pattern
{% endcomment %}
{% assign current = "" %}
{% assign count = 0 %}
{% assign tag_counts = "" %}

{% for tag in tag_array %}
  {% if tag != "" %}
    {% if tag != current %}
      {% if current != "" %}
        {% assign tag_counts = tag_counts | append: current | append: "|" | append: count | append: "," %}
      {% endif %}
      {% assign current = tag %}
      {% assign count = 1 %}
    {% else %}
      {% assign count = count | plus: 1 %}
    {% endif %}
  {% endif %}
{% endfor %}

{% if current != "" %}
  {% assign tag_counts = tag_counts | append: current | append: "|" | append: count | append: "," %}
{% endif %}

{% comment %}
  Step 3: Build arrays grouped by counts
  Liquid cannot sort by value directly, so we will manually collect by count descending
{% endcomment %}
{% assign tag_count_pairs = tag_counts | split: "," | sort_natural | reverse %}

<ul>
{% for pair in tag_count_pairs %}
  {% if pair != "" %}
    {% assign parts = pair | split: "|" %}
    <li>{{ parts[0] }} ({{ parts[1] }})</li>
  {% endif %}
{% endfor %}
</ul>
