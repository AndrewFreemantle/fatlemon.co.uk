---
title: 'Setting up Ubuntu on Digital Ocean'
author: Andrew Freemantle
layout: post
permalink: /2017/01/setting-up-ubuntu-on-digital-ocean/
tags:
  - Ubuntu
  - Linux
  - Digital Ocean
---

![]({{ site.imageurl }}2017/digital-ocean-header.png)

<span title="Digital Ocean - referral link, get $10 credit free!">[Digital Ocean](https://m.do.co/c/464a6866a39f)* </span> is an internet hosting service that makes it trivial to spin up virtual servers called Droplets. While the base Ubuntu image Droplets are configured for the job, there are a couple of extra steps I take with new Ubuntu Droplets that I'm documenting here as much for my own future reference as to [elicit your feedback](/2017/01/setting-up-ubuntu-on-digital-ocean/#post-comments) <i class="fa fa-smile-o text-warning"></i>


### SSH Keys

I'll typically create a new SSH key pair for a each Droplet. [Digital Ocean's community guide](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets) is comprehensive if you need a refresher or haven't done it before.

<i class="fa fa-terminal"></i>`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

### Droplet creation

After logging into Digital Ocean (or [signing up - use this link for an extra $10 <abbr title="United States Dollars">USD</abbr> credit](https://m.do.co/c/464a6866a39f)), we click <span class="btn btn-success" style="display:inline-block;padding:0 0.5em">Create Droplet</span> and follow the wizard.  
<br />

Here are the typical base settings I use:  
<table class="table table-bordered table-striped">
  <tr>
    <td style="white-space:nowrap"><strong>Distributions</strong></td>
    <td>Ubuntu, latest <abbr title="Long Term Maintenance">LTM</abbr>, x64</td>
  </tr>
  <tr>
    <td style="white-space:nowrap"><strong>Size</strong></td>
    <td>As per requirements (usually the smallest $5/mo)</td>
  </tr>
  <tr>
    <td style="white-space:nowrap"><strong>Datacenter region</strong></td>
    <td>Best to pick the one closest to the majority of our expected userbase. That might only be us <i class="fa fa-smile-o text-warning"></i></td>
  </tr>
  <tr>
    <td style="white-space:nowrap"><strong>Select additional options</strong></td>
    <td>As per requirements (I just Monitoring)</td>
  </tr>
  <tr>
    <td style="white-space:nowrap"><strong>Add your SSH Keys</strong></td>
    <td>Click <span class="btn btn-default" style="display:inline-block;padding:0 0.5em;background-color:#dfdfdf;color:#676767">New SSH Key</span> and paste in the public part of the SSH Key generated earlier</td>
  </tr>
  <tr>
    <td style="white-space:nowrap"><strong>Finalise and create</strong></td>
    <td>As per requirements</td>
  </tr>
</table>

Then we click <span class="btn btn-success" style="display:inline-block;padding:0 0.5em;">Create</span> and wait less than a minute while Digital Ocean performs its magic <i class="fa fa-magic"></i>


### Configuration
For convenience we can give our new Droplet a friendly SSH name by adding the following to our local `~/.ssh/config` file (I usually make this the same as the Droplet's name):

```
# ~/.ssh/config
...
Host {droplet-name}
    User root
    HostName {droplet-ip-address}
    IdentityFile "~/.ssh/{our-new-ssh-private-key}"
...
```

Now we can SSH into our new Ubuntu Droplet with  
<i class="fa fa-terminal"></i>`ssh {droplet-name}`

#### <i class="fa fa-globe"></i> Set the timezone
<i class="fa fa-terminal"></i>`dpkg-reconfigure tzdata`

#### <i class="fa fa-download"></i> Ensure all packages are up-to-date
<i class="fa fa-terminal"></i>`apt-get update; apt-get -y upgrade; apt-get -y clean`

#### <i class="fa fa-shield"></i> Configure automatic security patches (documentation [here](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) and [here](https://www.digitalocean.com/community/questions/auto-upgrade-packages-on-ubuntu))
<i class="fa fa-terminal"></i>`apt-get -y install unattended-upgrades; dpkg-reconfigure unattended-upgrades`  
Follow the prompts and accept the defaults.

#### <i class="fa fa-lock"></i> Lock SSH to keys-only
Edit `sshd_config` to prevent root SSH login with a password - change `PermitRootLogin` from `yes` to `without-password` like so:

```
# /etc/ssh/sshd_config
...
# Authentication:
LoginGraceTime 120
PermitRootLogin without-password
StrictModes yes
...
```

And finally, reboot the Droplet to ensure our settings are loaded, current and it comes back to us before we start installing or configuring our application stack of choice..  
<i class="fa fa-terminal"></i>`reboot`


Is there anything you'd add to this list of initial Ubuntu server setup steps? - [Please let us know in the comments](/2017/01/setting-up-ubuntu-on-digital-ocean/#post-comments)!
