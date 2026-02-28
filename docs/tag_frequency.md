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
  Step 2: Build frequency map
{% endcomment %}
{% assign tag_counts = {} %}

{% for tag in tag_array %}
  {% if tag != "" %}
    {% if tag_counts[tag] %}
      {% assign tag_counts = tag_counts | merge: {{ tag: tag_counts[tag] | plus: 1 }} %}
    {% else %}
      {% assign tag_counts = tag_counts | merge: {{ tag: 1 }} %}
    {% endif %}
  {% endif %}
{% endfor %}

{% comment %}
  Step 3: Convert frequency map into a sorted array of "tag|count"
{% endcomment %}
{% assign sorted_tags = "" %}

{% for tag in tag_counts %}
  {% assign sorted_tags = sorted_tags | append: tag[0] | append: "|" | append: tag[1] | append: "," %}
{% endfor %}

{% assign sorted_tags_array = sorted_tags | split: "," | sort_natural %}

{% comment %}
  Step 4: Sort by count descending manually
{% endcomment %}
{% assign sorted_tags_final = "" %}
{% assign temp = sorted_tags_array %}

{% comment %}
  Liquid doesn’t support complex sorting by value, so we can use a trick: build an array of counts descending
{% endcomment %}
{% assign counts = "" %}
{% for pair in temp %}
  {% if pair != "" %}
    {% assign parts = pair | split: "|" %}
    {% assign counts = counts | append: parts[1] | append: "," %}
  {% endif %}
{% endfor %}

{% assign counts_array = counts | split: "," | uniq | sort_natural | reverse %}

<ul>
{% for count in counts_array %}
  {% for pair in temp %}
    {% if pair != "" %}
      {% assign parts = pair | split: "|" %}
      {% if parts[1] == count %}
        <li>{{ parts[0] }} ({{ parts[1] }})</li>
      {% endif %}
    {% endif %}
  {% endfor %}
{% endfor %}
</ul>
