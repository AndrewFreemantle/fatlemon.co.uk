---
title: "LAN Speed Record: WiFi vs Powerline vs Ethernet"
permalink: /2025/06/lan-speed-record-wi-fi-vs-powerline-vs-ethernet/
excerpt: "WiFi 5 doesn't have the stamina for prolonged gaming sessions, the latest Powerline is much better but I soon hit the limit of my home's electrical system"
tags:
  - LAN
  - Internet Speed
  - WiFi
  - Powerline
  - Ethernet
---

![alt text here]({{ site.imageurl }}2025/lan-speed-record/lan-speed-record-header.jpg)
<figcaption>Image generated with <a href="https://gemini.google.com">Google Gemini 2.5 Pro</a></figcaption>

I was recently troubleshooting poor home network performance while online gaming with friends. Initially my connection over [WiFi 5](https://en.wikipedia.org/wiki/IEEE_802.11ac-2013) was superb, but after a short amount of time games would stutter and then degrade from un-enjoyable to unplayable.

I resorted to running an ethernet cable the length of the house which isn't a permanent solution so before making holes in walls and [pulling CAT6 cable](https://en.wikipedia.org/wiki/Category_6_cable), I thought I'd see what the [latest Powerline adaptors](https://www.tp-link.com/uk/home-networking/powerline/pg2400p-kit/) could do.

[Fibre to my home](https://en.wikipedia.org/wiki/Fiber_to_the_x) is contracted to a maximum of **82Mbps** (10.2MB/s)

| *Max: 82Mbps* | <i class="fa fa-fw fa-ethernet"></i> Ethernet | <i class="fa fa-fw fa-bolt"></i> Powerline | <i class="fa fa-fw fa-wifi"></i> WiFi 5 |
|--|--:|--:|--:|
| Download (Mbps) | <span class="text--success">88.57</span> | <span class="text--warning">88.02</span> | <span class="text--danger">76.96</span> |
| Upload (Mbps) | <span class="text--warning">86.82</span> | <span class="text--success">87.47</span> | <span class="text--danger">86.61</span> |
| Ping (ms) | <span class="text--success">7</span> | <span class="text--warning">10</span> | <span class="text--danger">12</span> |
| Download Latency (ms) | <span class="text--success">64</span> | <span class="text--warning">77</span> | <span class="text--danger">103</span> |
| Upload Latency (ms) | <span class="text--success">25</span> | <span class="text--warning">35</span> | <span class="text--danger">65</span> |

> Speed tests were conducted sequentially, and there are other devices on the home network which would affect the results.

**<i class="fa fa-fw fa-bolt"></i> Powerline clearly wins.**

While all 3 managed to utilise all of the available bandwidth, the latency of Powerline is so close to Ethernet that the convenience over a trip-hazard trailing cable makes it the clear winner. After some extensive gaming session testing it performs consistently too - beating WiFi 5.

### Update:
Our fibre contract renewed and for the same price the maximum is now **500Mbps** (62.5MB/s) 🤩

Is Powerline able to keep up? It has a theoretical maximum of *1482Mbps (185.2MB/s)* so even with the caveats of interference and electrical cabling quality can it deliver?

| *Max: 500Mbps* | <i class="fa fa-fw fa-ethernet"></i> Ethernet | <i class="fa fa-fw fa-bolt"></i> Powerline | <i class="fa fa-fw fa-wifi"></i> WiFi 5 |
|--|--:|--:|--:|
| Download (Mbps) | <span class="text--success">540.02</span> | <span class="text--danger">337.42</span> | <span class="text--warning">410.76</span> |
| Upload (Mbps) | <span class="text--success">535.14</span> | <span class="text--danger">329.44</span> | <span class="text--warning">417.59</span> |
| Ping (ms) | <span class="text--success">15</span> | <span class="text--success">16</span> | <span class="text--warning">19</span> |
| Download Latency (ms) | <span class="text--success">54</span> | <span class="text--danger">157</span> | <span class="text--warning">65</span> |
| Upload Latency (ms) | <span class="text--success">20</span> | <span class="text--danger">156</span> | <span class="text--warning">35</span> |

Nope! Seems I've hit a Powerline electrical wiring limit some 23% of the theoretical, and 67% of my home's fibre maximum 😕

I suspect, given the much higher latency figures that I'm saturating the line and causing [bufferbloat](https://en.wikipedia.org/wiki/Bufferbloat).[^1]

That said, even though WiFi comes in second to Ethernet based on speed and latency, it still can't sustain those numbers for even the shortest of demanding gaming sessions, and for all that Powerline can't transfer the full 500+Mbps fibre speed to where I want it in my home, 337Mbps is still **3.8⨉** more than 88Mbps.

---

[^1]: The way I resolved the Powerline bufferbloat was to use a [software tool](https://stackoverflow.com/a/56595105/5662) to arbitrarily limit speeds system-wide. Using my previous testing results as a guide, I settled on 300Mbps (37.5MB/s) which gives latencies of 30ms 🤓
