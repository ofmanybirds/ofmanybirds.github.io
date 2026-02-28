---
layout: default
---

<p>
  A non-specific list of things we happen to find interesting.
  These are generally not endorsements, and the exceptions are tagged as such.
</p>

{% assign grouped = site.data.links | group_by: "category" | sort_natural: "name" %}

{% for group in grouped %}
<section>
  <h2>{{ group.name | capitalize }}</h2>
  <ul>

    {% assign sorted_items = group.items | sort_natural: "title" %}

    {% for item in sorted_items %}
    <li>
      <a href="{{ item.url }}">{{ item.title }}</a>{% if item.note %} — {{ item.note }}{% endif %}

      {% if item.tags and item.tags.size > 0 %}

        {% assign content = "" %}
        {% assign epistemic = "" %}
        {% assign form = "" %}
        {% assign cw = "" %}

        {% for tag in item.tags %}
          {% if site.tag_schema.epistemic contains tag %}
            {% assign epistemic = epistemic | append: tag | append: "," %}
          {% elsif site.tag_schema.form contains tag %}
            {% assign form = form | append: tag | append: "," %}
          {% elsif site.tag_schema.cw contains tag %}
            {% assign cw = cw | append: tag | append: "," %}
          {% else %}
            {% assign content = content | append: tag | append: "," %}
          {% endif %}
        {% endfor %}

        {% assign content = content | split: "," | sort_natural | join: ", " | strip %}
        {% assign epistemic = epistemic | split: "," | sort_natural | join: ", " | strip %}
        {% assign form = form | split: "," | sort_natural | join: ", " | strip %}
        {% assign cw = cw | split: "," | sort_natural | join: ", " | strip %}

        {% assign output = "" %}

        {% if content != "" %}
          {% assign output = content %}
        {% endif %}

        {% if epistemic != "" %}
          {% if output != "" %}
            {% assign output = output | append: " | " | append: epistemic %}
          {% else %}
            {% assign output = epistemic %}
          {% endif %}
        {% endif %}

        {% if form != "" %}
          {% if output != "" %}
            {% assign output = output | append: " | " | append: form %}
          {% else %}
            {% assign output = form %}
          {% endif %}
        {% endif %}

        {% if cw != "" %}
          {% if output != "" %}
            {% assign output = output | append: " | " | append: cw %}
          {% else %}
            {% assign output = cw %}
          {% endif %}
        {% endif %}

        {% if output != "" %}
          <small>[{{ output }}]</small>
        {% endif %}

      {% endif %}
    </li>
    {% endfor %}

  </ul>
</section>
{% endfor %}
