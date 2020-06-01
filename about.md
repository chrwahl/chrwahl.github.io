---
layout: default
title: About
permalink: /about/
---

<section>
  {% assign author = site.authors | where: 'short_name', page.author | first %}
  {% if author %}
    {{ author.about | markdownify}}
  {% endif %}
</section>
