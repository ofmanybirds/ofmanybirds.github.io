---
layout: default
---

<p>
  A non-specific list of things we happen to find interesting. These are generally not endorsements, and the exceptions are tagged as such.
</p>

{% assign grouped = site.data.links | group_by: "category" | sort_natural: "name" %}

{% for group in grouped %}
<section>
  <h2>{{ group.name | capitalize }}</h2>
  <ul>

    {% assign sorted_items = group.items | sort_natural: "title" %}

    {% for item in sorted_items %}
    <li>
      <a href="{{ item.url }}">{{ item.title }}</a>

      {% if item.tags and item.tags.size > 0 %}

        {% assign grouped_tags = "" | split: "" %}
        {% assign order = site.tag_schema.order %}

        {% comment %}
          Build a hash of category → collected tags
        {% endcomment %}

        {% assign content = "" | split: "" %}
        {% assign epistemic = "" | split: "" %}
        {% assign form = "" | split: "" %}
        {% assign cw = "" | split: "" %}

        {% for tag in item.tags %}
          {% assign classified = false %}

          {% for category in site.tag_schema.categories %}
            {% assign cat_name = category[0] %}
            {% assign cat_tags = category[1] %}

            {% if cat_tags contains tag %}
              {% if cat_name == "epistemic" %}
                {% assign epistemic = epistemic | push: tag %}
              {% elsif cat_name == "form" %}
                {% assign form = form | push: tag %}
              {% elsif cat_name == "cw" %}
                {% assign cw = cw | push: tag %}
              {% endif %}
              {% assign classified = true %}
            {% endif %}
          {% endfor %}

          {% unless classified %}
            {% assign content = content | push: tag %}
          {% endunless %}
        {% endfor %}

        {% assign content = content | sort_natural %}
        {% assign epistemic = epistemic | sort_natural %}
        {% assign form = form | sort_natural %}
        {% assign cw = cw | sort_natural %}

        {% assign final_groups = "" | split: "" %}

        {% for type in order %}
          {% if type == "content" and content.size > 0 %}
            {% assign final_groups = final_groups | push: content %}
          {% elsif type == "epistemic" and epistemic.size > 0 %}
            {% assign final_groups = final_groups | push: epistemic %}
          {% elsif type == "form" and form.size > 0 %}
            {% assign final_groups = final_groups | push: form %}
          {% elsif type == "cw" and cw.size > 0 %}
            {% assign final_groups = final_groups | push: cw %}
          {% endif %}
        {% endfor %}

        {% if final_groups.size > 0 %}
          <small>
            (
            {% for group_tags in final_groups %}
              {% for tag in group_tags %}
                {{ tag }}{% unless forloop.last %}, {% endunless %}
              {% endfor %}
              {% unless forloop.last %} | {% endunless %}
            {% endfor %}
            )
          </small>
        {% endif %}

      {% endif %}

      {% if item.note %}
        <div class="link-note">
          {{ item.note }}
        </div>
      {% endif %}

    </li>
    {% endfor %}

  </ul>
</section>
{% endfor %}
