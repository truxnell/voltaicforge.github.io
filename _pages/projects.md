---
layout: archive
permalink: /projects/
title: "Projects"
author_profile: true
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/raspi_squeezebox_player_and_app.jpg
---

{% capture wanted_type %}project{% endcapture %}

## Projects

Here are a collection of projects I am either working on, or completed


<div class="grid__wrapper">
  {% for post in site.collections %}
    {% if post.type == wanted_type %}
      {% include archive-single.html type="grid" %}
	{% endif %}    
  {% endfor %}
</div>