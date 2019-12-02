---
layout: page
title: Archive
---

## Chronological

{% for post in site.posts %}
{% assign currentDate = post.date | date: "%Y" %}
{% if currentDate != myDate %}

### {{ currentDate }}

{% assign myDate = currentDate %}
{% endif %}

* [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %-d" }}

{% endfor %}
