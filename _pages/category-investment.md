---
title: "investment"
layout: archive
permalink: /investment
author_profile: true
sidebar:
    nav: "sidebar-category"
---

{% assign posts = site.categories.investment %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}