---
title: 'Creating Thumbnails for the Synology DSM PhotoStation'
author: Andrew Freemantle
layout: post
permalink: /2016/12/creating-thumbnails-for-the-synology-dsm-photostation/
redirect_from:
  - /synothumbs/
tags:
  - Synology
  - Synology DSM
  - PhotoStation
  - Linux
  - Python
---

Having updated my Synology NAS box to the latest Disk Station Manager (DSM) - version 6.0.2 as of December 2016 - I read that the PhotoStation thumbnail filenames had changed, and it now generates fewer of them which takes less time and saves space. Given that we've [somewhere north of 105k photos and videos](https://twoyeartrip.com/blog/2015/05/two-years-of-travel-in-numbers-stats-round-up/), it would take the little 1.6GHz ARM CPU in the Synology *weeks* (if not *months*) to recreate them so I started looking for a faster way.

After a brief search I found the sterling work of Matthew Phillips' [`synothumb` script](https://www.phillips321.co.uk/2012/04/08/creating-thumbnails-for-the-synology-diskstation-photostation/), which has been [tweaked a few times](https://github.com/phillips321/synothumbs/network) most recently by one [Andrew Harris](https://www.drew-harris.com) and it's his version I started with.


### Prerequisites

* <i class="fa fa-fw fa-server"></i> Synology NAS with PhotoStation installed
* <i class="fa fa-fw fa-laptop"></i> Any PC, Netbook or Laptop that's more powerful than our Synology NAS, and that we don't mind leaving running for long periods of time (possibly days, depending on fast it is and how many photos we have..), and ideally with a wired LAN connection
* <i class="fa fa-fw fa-linux"></i> A Linux install or a live CD - I recommend [the latest Linux Mint Cinnamon](https://linuxmint.com/download.php)
* <i class="fa fa-fw fa-file-code-o"></i> An executable copy of the `synothumb` Python script in our linux home directory..

<i class="fa fa-terminal"></i>`curl -o synothumb.py https://raw.githubusercontent.com/AndrewFreemantle/synothumbs/master/synothumb.py`

<i class="fa fa-terminal"></i>`chmod +x synothumb.py`

### 1. A little Synology configuration..
First we have to configure the NFS settings in the Synology DSM - log in as 'Admin', open up the **Package Center** and disable **PhotoStation** - it's in the Action menu..

![]({{ site.imageurl }}2016/synothumbs-dsm-disable-photostation.png)
<p class="wp-caption-text"></p>

Next we need to enable NFS, which we'll find in the **Control Panel** under **File Services**.. we tick the **Enable NFS** box if it isn't already and then click <span class="btn btn-info" style="display:inline-block;padding:0 1.5em;">Apply</span>

![]({{ site.imageurl }}2016/synothumbs-dsm-enable-nfs.png)
<p class="wp-caption-text"></p>

And then we need to configure it.. still in the **Control Panel**, this time in **Shared Folder** (just above File Services), we select the **photo** folder and click the <span class="btn btn-default" style="display:inline-block;padding:0 1.5em;">Edit</span> - in the 'Edit Shared Folder photo' window, we need the last tab called **NFS Permissions** where we need to <span class="btn btn-default" style="display:inline-block;padding:0 1.5em;">Create</span> a new Read/Write privilege for the IP address of our Linux client. Click <span class="btn btn-info" style="display:inline-block;padding:0 1.5em;">Apply</span> and then <span class="btn btn-info" style="display:inline-block;padding:0 1.5em;">OK</span>

![]({{ site.imageurl }}2016/synothumbs-dsm-configure-nfs-for-photo-shared-folder.png)
<p class="wp-caption-text"></p>

### 2. Connecting from Linux to our Synology

Now to our Linux client.. first we need NFS installed

<i class="fa fa-terminal"></i>`sudo apt-get install nfs-common`

Then we can check if we've configured NFS properly by running

<i class="fa fa-terminal"></i>`showmount -e {synology-ip-address}`

If it returns the IP address of our Linux client then we can mount the photo share like so..

<i class="fa fa-terminal"></i>`cd; mkdir mnt_photo`

<i class="fa fa-terminal"></i>`sudo mount -t nfs {synology-ip-address}:/volume1/photo mnt_photo/`


### 3. (Optional) Remove existing thumbnails

Matthew's `synothumbs` script skips media that already has thumbnails, so if we want to recreate them all rather than generate any missing ones we just need to run

<i class="fa fa-terminal"></i>`find mnt_photo/ -type d -name "@eaDir" -exec rm -rf {} \;`


### 4. Generating thumbnails

<i class="fa fa-terminal"></i>`./synothumb.py mnt_photo/`


### 5. Monitoring progress..

My contribution to the script is bit of error handling that outputs a list of bad files, so as well as watching `synothumb.py` whizzing through our photos, I have another terminal window with a tail going..

<i class="fa fa-terminal"></i>`touch mnt_photo/synothumb-bad-file-list.txt`

<i class="fa fa-terminal"></i>`tail -f mnt_photo/synothumb-bad-file-list.txt`

I also like to have the **Activity Monitor** with the graphical Resources tab running..

![]({{ site.imageurl }}2016/synothumbs-linux-mint-system-monitor.png)
<p class="wp-caption-text"></p>


### 6. Finishing up..

<i class="fa fa-clock-o"></i> *A little while later..*

Once the `synothumb.py` script has finished, we can unmount the share..

<i class="fa fa-terminal"></i>`sudo umount mnt_photo`

.. and in the DSM we then

* Remove the **NFS Permission** we created (**Control Panel** > **Shared Folder** > **photo** > **NFS Permissions**)
* Disable the **NFS Service** if we enabled it
* Restart the PhotoStation package (**Package Center** > **PhotoStation** > **Actions** > **Run**)


**Done**  <i class="fa fa-smile-o text-warning"></i>

Huge thanks to [Matthew Phillips](https://www.phillips321.co.uk/2012/04/08/creating-thumbnails-for-the-synology-diskstation-photostation/) for creating the `synothumbs` script in the first place, to all the forkers for their tweaks and to [Francesco Pipia](http://twitter.com/fpipia) for [his comment](https://www.phillips321.co.uk/2012/04/08/creating-thumbnails-for-the-synology-diskstation-photostation/#comment-373) that helped me get the NFS connection working! *Grazie mille!*
