---
layout: archive
permalink: /guides/
title: "Guides"
author_profile: true
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/steam_bigpicutre_retrogaming.png
---

{% capture wanted_type %}guide{% endcapture %}

## Guides

Here are a collection of guides I have posted


<div class="grid__wrapper">
  {% for post in site.collections %}
    {% if post.type == wanted_type %}
      {% include archive-single.html type="grid" %}
	{% endif %}    
  {% endfor %}
</div>