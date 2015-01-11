---
layout: page
title: 筆記
permalink: /notes/
---

{% for note in site.documents %}
  [{{ note.title }}]({{ site.baseurl }}{{ note.url }})
{% endfor %}
