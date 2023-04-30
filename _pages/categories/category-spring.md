---
title: "spring"
layout: archive
permalink: categories/spring
---

{% assign posts = site.categories.spring %}
{% for post in posts %}
    {% include archive-single.html type=page.entries_layout %}
{% endfor %}