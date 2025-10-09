---
layout: page
title: Cheatsheets
permalink: /cheatsheets/
---

Quick reference guides and code snippets I use frequently.

{% for cheatsheet in site.cheatsheets %}
- [{{ cheatsheet.title }}]({{ cheatsheet.url }}){% if cheatsheet.description %} — {{ cheatsheet.description }}{% endif %}
{% endfor %}
