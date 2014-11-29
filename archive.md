---
layout: page
title: Archive
---

{% for post in site.posts %}
 {% unless post.draft %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
 {% unless post.draft %}
{% endfor %}
