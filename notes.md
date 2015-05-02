---
layout: page
title: 笔记
permalink: /notes/
---

{% for note in site.documents %}
  [{{ note.title }}]({{ site.baseurl }}{{ note.url }})
{% endfor %}
