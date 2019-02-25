---
layout: archive
permalink: /year-archive/
title: "Posts by Year"
author_profile: true
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/modified_speakers_with_raspi.jpg
---

<div class="grid__wrapper">
  {% for post in site.posts %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>