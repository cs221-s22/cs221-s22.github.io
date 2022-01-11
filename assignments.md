---
layout: default
permalink: /assignments/
---

# Assignments

{% assign assignments_by_date = site.assignments | sort: "due" %}
{% for a in assignments_by_date -%}
    <p>
        <a href="{{ a.permalink | relative_url }}">{{ a.title }}</a>
    </p>
{%- endfor -%}