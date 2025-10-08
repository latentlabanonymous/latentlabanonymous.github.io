---
title: "excel"
layout: archive
permalink: /excel
author_profile: true
sidebar:
    nav: "sidebar-category"
---

{% assign posts = site.categories.excel %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}