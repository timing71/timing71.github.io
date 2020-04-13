---
layout: page
group: reference
title: Constants and enumerations
---

{% for constant in site.data.constants %}

## {{ constant[0] }}

{% assign members = constant[1] %}

{% for member in members %}
  {% if member.first %}
- **{{ member[0] }}**:
  {% if member[1].first %}
    {% for item in member[1] %}
  - {{ item }}
    {% endfor %}
  {% else %}`{{ member[1] }}`
  {% endif %}
{% else %}
- **{{ member }}**
{% endif %}

{% endfor %}
{% endfor %}
