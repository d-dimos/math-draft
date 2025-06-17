---
layout: default
title: Topics
---

# Topics

Here are all available topics:

{% assign topic_pages = site.pages | where_exp: "page", "page.dir == '/topics/' and page.name != 'index.md'" %}
{% if topic_pages.size > 0 %}
{% for page in topic_pages %}
- [{{ page.title }}]({{ site.baseurl }}{{ page.url }})
{% endfor %}
{% else %}
- [Differential Dynamic Programming]({{ site.baseurl }}/topics/ddp/)
{% endif %}

[← Back to Home]({{ site.baseurl }}/)
