---
layout: page
title: Projects
permalink: /projects/
nav_order: 4
---

<ul>
{% assign items = site.projects | sort: "date" | reverse %}
{% for p in items %}
  <li style="margin-bottom:1rem;">
    <a href="{{ p.url | relative_url }}"><strong>{{ p.title }}</strong></a>
    {% if p.date %} — {{ p.date | date: "%B %Y" }}{% endif %}
    {% if p.stack %} · <em>{{ p.stack }}</em>{% endif %}<br/>
    {{ p.summary | default: p.excerpt }}
    {% if p.links %}
      <br/>
      {% if p.links.code %}[Code]({{ p.links.code }}){% endif %}
      {% if p.links.live %} · [Live]({{ p.links.live }}){% endif %}
    {% endif %}
  </li>
{% endfor %}
</ul>
