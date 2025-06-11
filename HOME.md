---
title: "HOME"
layout: default
permalink: /home
---

## Recent Posts...

{% for post in site.categories.posts limit:6 %}
  - [{{post.title}}]({{post.url}})
{% endfor %}
[See more...](/blog)
