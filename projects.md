---
layout: page
title: Projects
permalink: /projects/
nav_order: 3
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
      {% assign code_link = "" %}
      {% assign live_link = "" %}
      {% if p.links.code %}{% assign code_link = "[Code](" | append: p.links.code | append: ")" %}{% endif %}
      {% if p.links.live %}{% assign live_link = "[Live](" | append: p.links.live | append: ")" %}{% endif %}
      {% if code_link and live_link %}
        {% assign combined_links = code_link | append: " · " | append: live_link %}
      {% elsif code_link %}
        {% assign combined_links = code_link %}
      {% elsif live_link %}
        {% assign combined_links = live_link %}
      {% endif %}
      {% if combined_links %}
        <span>{{ combined_links | markdownify | strip_newlines }}</span>
      {% endif %}
    {% endif %}
  </li>
{% endfor %}
</ul>
