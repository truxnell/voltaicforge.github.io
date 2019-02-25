---
title: "Nexus 6P Pocket Dial woes"

excerpt: "Frustrations with 6P pocketdials and the settings that need to be off to resolve"
header:
    overlay_filter: 0.5
    overlay_image: assets/images/nexus-6p-bend.png
    teaser: assets/images/nexus-6p-bend.png
categories:
   - Consumer-Electronics
tags: 
   - Nexus 6P
---

## Nexus 6P keeps pocket dialing

I love my Nexus 6P. It’s a fantastic android phone and easily the best phone I’ve ever owned (despite the almost excellent experience with my Samsung Galaxy S4). The Nexus 6P is perfect for someone who wants a non-bloated OS/ROM but also still excellent hardware.

Also, the phone is encouraging me to be very social. Like, calling my friends from my pocket whilst there at work. Sorry, Crossy.

I’ve been scratching my head and finally found out the root settings that were causing the pocket dialing.

## Fixing pocket dialing

Finally figured out the pocket dialing settings that were causing me the woes. Many people complain about ambient display waking up the phone in your pocket, and then from there the phone getting random touches from your leg and thus pocket dialing.

In my case, that wasn’t actually the whole story. My issue was actually the `Tap to wake` being set to on. This setting wakes the device if it detects a double-tap on the screen whilst it’s off. So yeah, pocket dial city - I don’t remember my S4 being this touchy feely on the screen.

Ensure both are off and you should be fine.



{% capture imagesrc %}6P_settings_display_dbltap_ambient.png{% endcapture %}
{% capture imagetitle %}Image of Nexus 6P settings that should be disabled to stop pocket dialing{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_portrait {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}



Unfortunately, Ambient Display has been pretty poor on Andriod so far. It should be quite good - but I’ve found it difficult to use effectively. It is quite difficult to move it enough if it’s just sitting on your desk to activate ambient. It seemed to display every time you didn’t care about it - and none of the times it did.

Time (and patches) will tell if this feature is built into anything as good as Moto Display.
