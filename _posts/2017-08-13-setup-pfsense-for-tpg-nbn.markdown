---
title: Setup pfSense for TPG NBN
excerpt: "An adventure of frustration with TPG NBN and the missing VLAN setting"
header:
    overlay_filter: 0.5
    overlay_image: "/assets/images/Code.jpg"
    teaser: "/assets/images/Code.jpg"
categories:
   - Homelab
tags: 
   - homelab
   - TPG
   - NBN
   - VLAN
---

## Setting up pfSense for TPG NBN

After much frustration, I have finially buggered off the terrible HG659 router that is provided with TPG NBN.

I had significant trouble getting pfSense to connect to the NBN even using the details on their site.  PPPoE should be reasonably simple, connect NBN box to pfSense WAN port, set to PPPoE and enter TPG username/password.

**Wrong!**

After quite some time I found that they use a VLAN setting, and without it being set right it would not connect at all.  Its not mentioned in their setting site, conspicuously.

{% capture imagesrc %}TPG_NBN_Settings.png{% endcapture %}
{% capture imagetitle %}TPG's NBN settigns with no VLAN setting mentioned{% endcapture %}	
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

I got mine working once I set the WAN port to be on a VLAN 2.

Create a new VLAN, under *Interface -> Assign -> VLANs -> Add*.  Setup on the WAN Interface (mine is `re0`) and with a VLAN tag of `2`.

{% capture imagesrc %}TPG_VLAN_2.png{% endcapture %}
{% capture imagetitle %}pfSense VLAN setting of 2 for TPG NBN{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

Add a PPP configuration under *Interface -> Assign -> PPP -> Add*.  Use Link type `PPPoE`, Link Interface of the VLAN you set up before.  Enter TPG username/password

{% capture imagesrc %}pfSense_PPP_Config.png{% endcapture %}
{% capture imagetitle %}pfSense PPP configuration{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

For WAN port, under *Interface -> WAN*, ensure we have `PPPoE` set for IP4 Configuration Type, and again your Username/Password

{% capture imagesrc %}pfsense_TPG_WAN_Settings.png{% endcapture %}
{% capture imagetitle %}pfsense TPG WAN Settings{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

Then under *Interface -> Assign*, set the WAN network port to the PPPOE of the VLAN setup above.

{% capture imagesrc %}pfSesne_TPG_Interface_Assignment.png{% endcapture %}
{% capture imagetitle %}pfSense TPG setting of WAN interface to VLAN{% endcapture %}
<a href="/assets/images/{{ imagesrc }}">{% picture post_landscape {{ imagesrc }} alt="{{ imagetitle }}" title="{{ imagetitle }}" %}</a>
{: .text-center}

You should now have a working NBN setup under pfSense, and that locked down modem can be thrown in the trash where it belongs!


