---
layout: page
title: Cheatsheets
permalink: /cheatsheets/
nav_order: 3
---

Quick reference guides and code snippets I use frequently.

{% assign items = site.cheatsheets | sort: "title" %}
{% if items and items.size > 0 %}
{% for sheet in items %}

- [{{ sheet.title | escape }}]({{ sheet.url | relative_url }}){% if sheet.description %} — {{ sheet.description }}{% endif %}
{% endfor %}
{% else %}
- *(No cheat sheets yet.)* Add Markdown files to `_cheatsheets/` with front matter like:

  ```yaml
  ---
  title: "Big O Cheat Sheet"
  description: "Quick reference for time & space complexity"
  tags: [python, django]
  ---
  ```

{% endif %}
