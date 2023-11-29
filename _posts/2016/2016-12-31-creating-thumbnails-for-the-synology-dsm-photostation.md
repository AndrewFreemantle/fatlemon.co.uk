---
title: 'Creating Thumbnails for the Synology DSM PhotoStation'
permalink: /2016/12/creating-thumbnails-for-the-synology-dsm-photostation/
excerpt: "A method and script for using the power of desktop compute to generate or regenerate photo thumbnails for Synology's PhotoStation"
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

After a brief search I found the sterling work of [Matthew Phillips' `synothumb` script](https://www.phillips321.co.uk/2012/04/08/creating-thumbnails-for-the-synology-diskstation-photostation/), which has been [tweaked a few times](https://github.com/phillips321/synothumbs/network) most recently by one [Andrew Harris](https://www.drew-harris.com) and it's his version I started with.


### Prerequisites

* <i class="fa fa-fw fa-server"></i> Synology NAS with PhotoStation installed
* <i class="fa fa-fw fa-laptop"></i> Any PC, Netbook or Laptop that's more powerful than our Synology NAS, and that we don't mind leaving running for long periods of time (possibly days, depending on fast it is and how many photos we have..), and ideally with a wired Gigabit LAN connection
* <i class="fa fa-fw fa-linux"></i> A Linux install or a live CD - I recommend [the latest Linux Mint Cinnamon](https://linuxmint.com/download.php) (18.1 as of writing)
* <i class="fa fa-fw fa-file-code-o"></i> An executable copy of the `synothumb` Python script in our linux home directory..

<i class="fa fa-terminal"></i>`curl -o synothumb.py https://raw.githubusercontent.com/AndrewFreemantle/synothumbs/master/synothumb.py`

<i class="fa fa-terminal"></i>`chmod +x synothumb.py`

### 1. A little Synology configuration..
First we have to configure the NFS settings in the Synology DSM - log in as 'Admin', open up the **Package Center** and disable **PhotoStation** - it's in the Action menu..

![]({{ site.imageurl }}2016/synothumbs-dsm-disable-photostation.png)
<figcaption></figcaption>

Next we need to enable NFS, which we'll find in the **Control Panel** under **File Services**.. we tick the **Enable NFS** box if it isn't already and then click <span class="btn btn--info" style="display:inline-block;padding:0 1.5em;">Apply</span>

![]({{ site.imageurl }}2016/synothumbs-dsm-enable-nfs.png)
<figcaption></figcaption>

And then we need to configure it.. still in the **Control Panel**, this time in **Shared Folder** (just above File Services), we select the **photo** folder and click the <span class="btn btn--primary" style="display:inline-block;padding:0 1.5em;">Edit</span> - in the 'Edit Shared Folder photo' window, we need the last tab called **NFS Permissions** where we need to <span class="btn btn--primary" style="display:inline-block;padding:0 1.5em;">Create</span> a new Read/Write privilege for the IP address of our Linux client. Click <span class="btn btn--info" style="display:inline-block;padding:0 1.5em;">Apply</span> and then <span class="btn btn--info" style="display:inline-block;padding:0 1.5em;">OK</span>

![]({{ site.imageurl }}2016/synothumbs-dsm-configure-nfs-for-photo-shared-folder.png)
<figcaption></figcaption>

### 2. Connecting from Linux to our Synology

Now to our Linux client.. first we need NFS and the video libraries installed. In a Terminal we run

<i class="fa fa-terminal"></i>`sudo apt-get install nfs-common ffmpeg libav-tools ufraw`

Then we can check if we've configured NFS properly by running

<i class="fa fa-terminal"></i>`showmount -e {synology-ip-address}`

If it returns the IP address of our Linux client then we can mount the photo share like so..

<i class="fa fa-terminal"></i>`cd; mkdir mnt_photo`

<i class="fa fa-terminal"></i>`sudo mount -t nfs {synology-ip-address}:/volume1/photo mnt_photo/`


### 3. (Optional) Remove existing thumbnails

Matthew's `synothumbs` script skips media that already has thumbnails, so if we want to recreate them all rather than generate any missing ones we just need to run this first..

<i class="fa fa-terminal"></i>`find mnt_photo/ -type d -name "@eaDir" -exec rm -rf {} \;`


### 4. Generating thumbnails

<i class="fa fa-terminal"></i>`./synothumb.py mnt_photo/`


### 5. Monitoring progress..

As well at watching the image and video filenames fly past, I like to have the **Activity Monitor** with the graphical Resources tab running..

![]({{ site.imageurl }}2016/synothumbs-linux-mint-system-monitor.png){: .center-image }
<figcaption>Watching the System Monitor - if the CPU's aren't pegged at 100% then we need a faster network!</figcaption>


### 6. Finishing up..

<i class="fa fa-clock-o"></i> *A little while later..*

Once the `synothumb.py` script has finished, we need to modify the ownership and permissions of the thumbnails we've generated. For this we need an SSH or terminal / telnet session on our Synology, then we can issue the following:

<div class="notice--warning">
  <i class="fa fa-fw fa-sticky-note text--warning"></i>Note: If we've logged in with Terminal / telnet, and our prompt shows us as the <code>admin</code> user, we can issue the following command to become <code>root</code> (it'll prompt us for our Admin password again)<br/>

  <i class="fa fa-terminal"><span>admin@Synology#</span></i><code>sudo -i</code>
</div>

<i class="fa fa-terminal"><span>root@Synology#</span></i>`cd /volume1/photo`

<i class="fa fa-terminal"><span>root@Synology#</span></i>`find . -type d -name "@eaDir" -exec chown -R PhotoStation:PhotoStation {} \;`

<i class="fa fa-terminal"><span>root@Synology#</span></i>`find . -type d -name "@eaDir" -exec chmod -R 770 {} \;`

<i class="fa fa-terminal"><span>root@Synology#</span></i>`find . -type f -name "SYNOPHOTO_THUMB*" -exec chmod 660 {} \;`


Next we need to ensure that when the PhotoStation starts re-indexing that it can't re-create the thumbnails itself - that would *seriously* defeat the point of all of the work we've just done! Fortunately it's easy to do.. while we're in our SSH or terminal / telnet session we can point the existing thumbnail links to a simple script that just returns success!

<i class="fa fa-terminal"><span>root@Synology#</span></i>`cd /usr/syno/bin`

<i class="fa fa-terminal"><span>root@Synology#</span></i>`echo -e '#!/bin/bash\nexit 0' > fake-thumb.sh`

<i class="fa fa-terminal"><span>root@Synology#</span></i>`chmod +x fake-thumb.sh`

Next we'll just make a note of the current symbolic links:

* Photos: <i class="fa fa-terminal"><span>root@Synology#</span></i>`la -la convert-thumb`
* Videos: <i class="fa fa-terminal"><span>root@Synology#</span></i>`ls -la ffmpeg-thumb`

Which should yield the following results:

* `convert-thumb -> /usr/bin/convert`
* `ffmpeg-thumb -> /usr/bin/ffmpeg`

<div class="notice--danger">
  <i class="fa fa-fw fa-exclamation text--danger"></i><strong>Important!</strong> Make a note of these paths <strong>if they're different to the one's above</strong> as we'll need them later!
</div>

Now we can point these symbolic links to out `fake-thumb.sh` script:

<i class="fa fa-terminal"><span>root@Synology#</span></i>`ln -s fake-thumb.sh convert-thumb`

<i class="fa fa-terminal"><span>root@Synology#</span></i>`ln -s fake-thumb.sh ffmpeg-thumb`

<br />
Back to our Linux thumbnail processing box, we can unmount the drive..

<i class="fa fa-terminal"></i>`sudo umount mnt_photo`

.. and in the DSM we then

* Remove the **NFS Permission** we created (**Control Panel** > **Shared Folder** > **photo** > **NFS Permissions**)
* Disable the **NFS Service** if we enabled it
* Restart the PhotoStation package (**Package Center** > **PhotoStation** > **Actions** > **Run**)

Once the PhotoStation package is back up and running it should automatically start re-indexing - that is, searching `/volume1/photo` for new photos. If we'd just copied a lot of new photos and videos onto our Synology box then *this process can take longer than the thumbnail generation!* - for every media file found it reads the file's [EXIF data](https://en.wikipedia.org/wiki/Exif) and writes it to local database.

We can check that the re-indexing has started (and kick it off it hasn't) by opening **PhotoStation** in our browser - `https://{synology-ip}/photo` - and logging in as Admin.

Then we choose **Settings** > **Photos** > and click <span class="btn btn--primary" style="display:inline-block;padding:0 1.5em;">Re-index</span>

![]({{ site.imageurl }}2016/synothumbs-photostation-re-index.png)
<figcaption>Ensuring that PhotoStation knows about any new photos and videos we've added by re-indexing</figcaption>

<br />

<i class="fa-regular fa-fw fa-clock"></i> When the PhotoStation's re-indexing has finished, we mustn't forget to revert the thumbnail symbolic links:

<i class="fa fa-terminal"><span>root@Synology#</span></i>`ln -s /usr/bin/convert /usr/syno/bin/convert-thumb`

<i class="fa fa-terminal"><span>root@Synology#</span></i>`ln -s /usr/bin/ffmpeg /usr/syno/bin/ffmpeg-thumb`

**Done** 🙂

Huge thanks to [Matthew Phillips](https://www.phillips321.co.uk/2012/04/08/creating-thumbnails-for-the-synology-diskstation-photostation/) for creating the `synothumbs` script in the first place, to all the forkers for their tweaks and to [Francesco Pipia](http://twitter.com/fpipia) for [his comment](https://www.phillips321.co.uk/2012/04/08/creating-thumbnails-for-the-synology-diskstation-photostation/#comment-373) that helped me get the NFS connection working! *Grazie mille!*
