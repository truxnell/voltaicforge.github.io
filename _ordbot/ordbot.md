---
title: "Ordbot"
permalink: /projects/ordbot/
header:
  overlay_filter: 0.5
  overlay_image: /assets/images/ordbot.jpg
---

# Ordbod Hardon build

I build my Ordbot back in December 2012, having been inerested in 3D printing for a few years.
I liked the concept of the original Reprap's, but to be honest I was a little put of by how fragile they looked, and I had some concerns about the slop they may have

Upon seeing a Ordbot Hadron at the hackerspace however, I was impressed. It is made out of Makerslide, and at the time in 2012 was far more rigid than other models I had seen.

I bought a Ordbot kit, a reprapdiscount RAMPS1.4 electronics set, and a Wades Extruder kit to round out the basics.

{% capture imagesrc %}ordbot_parts.jpg{% endcapture %}
{% capture imagetitle %}Ramps 1.4 kit{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

Assembly was interesting - There wasnt a heck of a lot of documentation, and I found myself looking at the engineering drawings and a whole bunch of forum posts of similar builds.

I spend a few days on the frame assembly. To be neat and tidy it seemed quite a few people drilled out holes in the frame to run cables in, which I proceeded to do.

{% capture imagesrc %}ordbot_frame_built.jpg{% endcapture %}
{% capture imagetitle %}Ordbot with assembled frame{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

After assembling the frame and running the stepper motor cables inside the frame, I took to wiring the eletronics.

Originally I had the Arduino/RAMPS combo bolted to the frame as below, which I later moved into its own case with fan to help with cooling the steppers. At the time, I didnt realise how much active cooling they required!

{% capture imagesrc %}Ordbot_wiring.jpg{% endcapture %}
{% capture imagetitle %}Ordbot with RAMPS wiring completed{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

Calibration was also a challenge, as was bed ahesion. Printing PLA on glass was all the rage in 2012, but I could never get it to stick well. Blue painters tape ended up being my goto.

{% capture imagesrc %}ordbot_early_prints.jpg{% endcapture %}
{% capture imagetitle %}Early prints from my Ordbot Handron{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

It wasn't until late 2015 until I completed Rev.2 of the Ordbot - upgraded with a Bulldog Extruder, E3D v6 hotend, printed case + fan for the RAMPS and a Full Graphic display for the LCD.

{% capture imagesrc %}ordbot_r2.jpg{% endcapture %}
{% capture imagetitle %}Rev.2 of my Ordbot Hadron printer, with printed case for RAMPS and full graphic display{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

The bulldog was far superiord to the printed wades, and the E3D hoten was amazing compared to the hotend I started with (Which I cant ever recall what it was now)

Later came a Raspi 3 with Octoprint, and a printed case for it which got tacked onto the back of the case. Having never designed the rear of the case for the amount of gear I was now trying to put in, I honestly never managed to get it neat and tidy. "Working" became a goal.

That said, after plenty of tuning, fiddling with the Marlin firmware, etc... I was able to get decent prints from it.

{% capture imagesrc %}ordbot_benchy.jpg{% endcapture %}
{% capture imagetitle %}Ordbot 3d print{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

It got some decent use - such as the 80 sculpture peices I did for a friend's wedding, as well as the love heart train carts I did for my step-daughters birthday, so during the arts and crafts activities we could have a moving train delivering the glitter and pens to everyone.

{% capture imagesrc %}ordbot_love_carriages.jpg{% endcapture %}
{% capture imagetitle %}Ordbot 3d print{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

However, with the arrival of the Prusa MK3s in the household, it is now fully dismantled, awaiting its upgrade to Rev3.
