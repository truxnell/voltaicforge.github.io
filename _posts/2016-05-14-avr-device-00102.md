---
title: "Error with device 0x000102 whilst programming AVR"

excerpt: "Chasing down a short to ground on the reset line that causes an initial correct device ID, but subsequent failures"
header:
    overlay_filter: 0.5
    overlay_image: assets/images/device_0x000102_avr_reset_short_teaser.png
    teaser: assets/images/device_0x000102_avr_reset_short_teaser.png
categories:
   - Electronics
tags: 
   - AVR
---


## Device is read correctly the first time but fails to be read on subsequent reads

I'm building an AVR target board for a few different flavours of AVR to make my life easier.

I had some _real fun_ trying to read the datasheet pins back to front whilst soldering down the sockets, and had to have more than one go at getting the ATMega328P socket wired up correctly.

I was using Atmel Studio 7 to read the device ID of various AVR chips I had on my board.  When suddenly it would work on the initial read after power up - but subsequent reads would fail with the device ID being returned reading `0x000102`

{% capture imagesrc %}device_0x000102_avr_reset_short.png{% endcapture %}
{% capture imagetitle %}Picture of Atmel Studio 7 returning devide 0x000102 after a reset line short to ground{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

## Reset line short to ground stopping reset line returning to high post-programming

I had introduced a short on the reset line to ground when fixing my wiring.  This was causing the reset pin to be low regardless of what the programmer did.

This makes sense - the initial read works OK as the reset line is LOW.  But the AVR chip never leaves programming mode until reset returns to HIGH.  This is the reason we add pullups to the reset line.

With the reset pullup being forced LOW all the time, the AVR never leaves programming mode, and a second programming attempt fails as the AVR is not reset back to the start of its programming phase.

