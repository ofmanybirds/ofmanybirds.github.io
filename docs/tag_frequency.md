---
layout: default
---

Tracker of how manytimes each tag is used. 

{% assign all_tags = "" %}

{% for item in site.data.links %}
  {% if item.tags %}
    {% for tag in item.tags %}
      {% assign all_tags = all_tags | append: tag | append: "," %}
    {% endfor %}
  {% endif %}
{% endfor %}

{% assign tag_array = all_tags | split: "," | sort_natural %}

<ul>
  {% assign current = "" %}
  {% assign count = 0 %}

  {% for tag in tag_array %}
    {% if tag != "" %}
      {% if tag != current %}
        {% if current != "" %}
          <li>{{ current }} ({{ count }})</li>
        {% endif %}
        {% assign current = tag %}
        {% assign count = 1 %}
      {% else %}
        {% assign count = count | plus: 1 %}
      {% endif %}
    {% endif %}
  {% endfor %}

  {% if current != "" %}
    <li>{{ current }} ({{ count }})</li>
  {% endif %}
</ul>
{% assign untagged_count = 0 %}
{% for item in site.data.links %}
  {% if item.tags == nil or item.tags.size == 0 %}
    {% assign untagged_count = untagged_count | plus: 1 %}
  {% endif %}
{% endfor %}

{% if untagged_count > 0 %}
<p>Untagged items: {{ untagged_count }}</p>
{% endif %}
