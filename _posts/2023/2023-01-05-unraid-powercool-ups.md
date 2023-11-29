---
title: 'Unraid + Powercool UPS'
permalink: /2023/01/unraid-powercool-ups/
excerpt: 'A short guide to configuring the Powercool UPS (Uninterruptible Power Supply) to work with Unraid'
tags:
  - Unraid OS
  - UPS
  - Uninterruptible Power Supply
  - Servers
---

<p style="text-align:center">
  <img alt="Powercool 850va UPS listing at Scan Computers" src="{{ site.imageurl }}2023/powercool-ups/powercool-850va-ups-at-scan-computers.png" />
</p>
<figcaption>Getting this generic UPS working with Unraid OS took a little work...</figcaption>

Unraid OS has had built-in support for uninterruptible power supplies (UPSs) for a while now, and it's been on my list to install one since setting up this very capable backup server.

However, it isn't as straightforward as plugging in the USB cable, so here's the configuration needed to get this Powercool 850va UPS working with Unraid OS.

### 1. Disable Unraid's built-in UPS support
*This disables the built-in `apcupsd` daemon which <span class="text--danger">doesn't</span> support our UPS*

In `Settings` <i class="fa fa-arrow-right"></i> `UPS Settings` ensure we have the following configuration:
- Start APC UPS daemon: **No**
- UPS cable: **Custom**
- UPS type: **Dumb**

![Disable Unraid's UPS Settings]({{ site.imageurl }}2023/powercool-ups/unraid-ups-settings.png)

### 2. Install the Network UPS Tools (NUT) Plugin
*This includes a collection of different UPS daemons, one of which <span class="text-success">does</span> support our UPS*

In `Plugins` <i class="fa fa-arrow-right"></i> `Install Plugin` paste the following URL and click `Install`

```
https://raw.githubusercontent.com/dmacias72/NUT-unRAID/master/plugin/nut.plg
```

![Install the Network UPS Tools (NUT) plugin]({{ site.imageurl }}2023/powercool-ups/unraid-install-nut-plugin.png)

### 3. Configure the new NUT Settings Plugin
In `Settings` we should now have a new `NUT Settings` option, and in there we need to make the following changes:

At the bottom of the page, in the `Config Editor`, select the `ups.conf` file and paste and save the following configuration:

![Configure the NUT plugin to see our Powercool UPS]({{ site.imageurl }}2023/powercool-ups/unraid-nut-config.png)

```
[ups]
driver = nutdrv_qx
port = auto
vendorid = 0001
productid = 0000
langid_fix = 0x0409
novendor
noscanlangid
```

Then back at the top of the NUT Settings page, start the service using our just updated manual config:

- Start Network UPS Tools service: **Yes**
- Enable Manual Config Only: **Yes**

![Enable the NUT plugin and set it to manual config]({{ site.imageurl }}2023/powercool-ups/unraid-nut-settings.png)

Once saved, Unraid now knows how to monitor our new Powercool UPS 🔌 🙌

*P.S. Don't forget to set the BIOS Power State to `Always On` so the UPS can turn the server back on after a power failure 🙂*

![Configure the NUT plugin to see our Powercool UPS]({{ site.imageurl }}2023/powercool-ups/unraid-nut-status.png)

---
Sources & Resources:
- [KP TechTips (youtube)](https://www.youtube.com/watch?v=h1V02gwfqkU)
- [Jammy B's forum post](https://forums.unraid.net/topic/104685-powercool-ups-compatible/?do=findComment&comment=1009430)
- [Network UPS Tools Hardware Compatibility List](https://networkupstools.org/stable-hcl.html)
- [nutdrv_qx driver man page](https://networkupstools.org/docs/man/nutdrv_qx.html)
