---
title: "W.E.P 853d clone soldering station"

excerpt: "A review and teardown of a temperature controlled, cheap chineese clone soldering/hot air rework station"
header:
    overlay_filter: 0.5
    overlay_image: /assets/images/wep_853d_overview.jpg
    teaser: assets/images/wep_853d_overview.jpg
categories:
   - Electronics
tags:
   - teardown
   - review
---
## Time for a upgrade, from cheap solder pen to.. cheap solder station. 

After living with a old non-temperature controlled soldering iron for years I decided it was time to upgrade.  I had been using a Goot PX232 for a long time, and before that some Jaycar $15 iron that was only just passable as a soldering iron.

Now, despite the fact I _should_ be getting a decent brand, I decided to try a cheap eBay clone.  I found a 'W.E.P 853D Soldering Station/Hot Air Gun/DC Power supply' on eBay, within Australia.  Few days later I have it in my grubby hands.

Ive been perfectly happy with the operation.  Ive had it now for a few months with no issues.  

The soldering iron has tips that fit over the cartridge so it heats up quite nicely.  It comes with a large variety of tips (conical, chisel, wedge) and sizes.  The iron holder is pretty lousy and the tip will hit the rear of the case and lean against it.  I've continued to use my old iron holder due to this.   Its such a relief to be using decent bits now after struggling along with conical for so long.

The iron heats up to 300&deg; C within 47 seconds, so on that side its not so great unfortunatly.

The hot air gun performs well and heats up in seconds.  It only operates when removed from its cradle (suspecting reed switch or similar) and heats up in about 4-5 seconds to 250&deg;C.  I used it to repair my [Samsung Galaxy S4 Repair][s4repairpost] that had broken soft keys in a recent firmware update and it worked well.  It was so much better than my old method of using a paint stripper as a reflow tool.
The hotair gun comes with a few tips to have wider/smaller airflow.

The power supply I only used a little bit but it was accurate to its displayed measurements.  I havent had much experience at measuring power supply ripple so will skip that for now, but I cant say ive noticed or seen much ripple on my oscilloscope.

## Teardown

Finially, thats out of the way!  I had to have a look see inside - would it be complete cheap Shenzhen market garbage?  Or would it be somewhat passable?  More importantly, was it a Yihua station with a different name like I suspected?

{% capture imagesrc %}wep_853d_overview.jpg{% endcapture %}
{% capture imagetitle %}WEP 853D soldering station view from front{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

Presentation was quite nice.  I much prefer the black and gold to the gawdy blue and yellow others have.  Buttons have a nice tactile feel to them and knobs are firmly stuck on and feel OK considering.  LCD is clear and bright.

{% capture imagesrc %}wep_853d_power.jpg{% endcapture %}
{% capture imagetitle %}WEP 853D soldering station transformer internal section{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}



I wont profress to be any good at a teardown, but the transformer seemed OK, earthed down to chassis with decent connectors.  Power transistor is well secured (housed on the rear of the chassis).  I know people have concerns about cheap chineese equipment such as this being a fire hazard - but I didnt see anything that concerned me.  Ive seen worse build quality from gear built in better countries and companies that was worse.

{% capture imagesrc %}wep_853d_mid.jpg{% endcapture %}
{% capture imagetitle %}WEP 853D soldering station mid section PC{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_portrait {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

PCB in the middle of the boad hosts some triacs, bridge rectifier, MMOC3041 optocouplers, and various passives.  Caps are noname ([Jwco]) garbage, but at least the triacs have heat sinks.  A single LM723 voltage regular sits on the board.  Soldering seemed perfecly fine, looks wave soldered (or at least reflowed)


{% capture imagesrc %}wep_853d_frontpanel.jpg{% endcapture %}
{% capture imagetitle %}WEP 853D soldering station mid section PCB{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

The control panel has two S3F94C4EZZ MCU's and two LED display drivers (TM1620 & TM1628).  Cant determine the manufacture for the MCU's, I get either a Samsung or Zilog part - I'll leave you to guess which I think is the more likely manufacturer.  Not much else happening there, soldering continues to look fine.  I cant see any post-production fixes or anything untoward dodgy - if I had been given the photos to look at without knowing where it came from it would look visually OK to me.

But wait a minute here - the control PCB has a silk-screen designator of 'YH852DV5A'.  Clearly the same control board as the Yihua model.  After some googling I found an [EEVBlog post](eevblogpost) regarding the Yihua which showed a photo with a identical control board, but what to me looked like a significantly worse power board.

In the meantime, whilst I wait to see if Shenzhen lets me down again I will continue to enjoy my nifty little soldering station. 

[eevblogpost]: http://www.eevblog.com/forum/reviews/new-toy-yihua-853dplus-rework-station/
[jwco]:http://www.jiaweicheng.com/EN
[s4repairpost]: http://127.0.0.1:4000/electronics/s4-soft-button-repair/