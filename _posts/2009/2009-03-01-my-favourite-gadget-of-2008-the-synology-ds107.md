---
title: 'My favourite gadget of 2008 – the Synology DS107'
author: Andrew Freemantle
layout: post
permalink: /2009/03/my-favourite-gadget-of-2008-the-synology-ds107/
tags:
  - Linux
  - Synology
---
Yep, I know it's March already <i class="fa fa-smile-o"></i> but back in December I picked up one of these because I was after some Network Attached Storage, and it has impressed me ever since.  This isn't a review as such, more a highlight of my favourite features and possibly some tips and tricks I've found..

![Synology DS107+]({{ site.imageurl }}2009/Synology-DS107.png)

My main reason for picking up the Synology DS-107+ was to have a large, always on, networked disk for multiple computer backups. It was easy to drop in a [Samsung Terrabyte disk](http://www.ebuyer.com/product/143288 "Samsung 1TB Spinpoint Disk - eBuyer.com") (as the box itself comes without a disk), [download the latest firmware](http://synology.com/enu/support/download.php?m=DS107 "Synology - DS107 Downloads Page"), and follow the simple setup wizard which formatted the disk and installed itself.

**Disk Station Manager 2.0**

Having read a few [reviews](http://www.smallnetbuilder.com/content/view/30157/75/ "SmallNetBuilder.com - Synology DS107e review") before I decided on it, I wasn't able to find one that talked about the new '[Disk Station Manager 2.0](http://synology.com/enu/products/features/index.php "Synology - Disk Station Manager 2.0 Features")' which Synology released in March 2008 (according to the [release notes](http://synology.com/enu/support/releaseNote/DS107.php "Synology - DS107 Firmware release notes") - this is the typical web-browser-based approach to device management we're used to seeing in ADSL routers, VoIP devices and NAS boxes.

Synology have included everything and the kitchen sink in thiers - it's all easily findable and configurable, including full 'root' access via telnet and ssh (the 'root' password is the same as the one you're asked for when initially setting up the 'admin' account - more on this a little later).

First, here's some stuff I've found really useful out of the box:

**Apple Time Machine backups**

I mainly use a Mac Mini at home, which I had been backing up with Time Machine to a Firewire External Hard Disk. I was pleased to find a [pretty straightforward guide](http://www.naschenweng.info/2008/07/15/os-x-time-machine-backup-to-synology-ds1 "Time Machine backup to Synology DS-107+") for using the Synology instead.

**Download Station 2.0 - HTTP, FTP, BitTorrent, NZB and eMule downloads**

Being able to set away downloads of fairly large files is pretty handy - I like to check out the latest enhancements to various Linux distributions so I've only had need for the HTTP options so far.

![Synology Download Station 2.0]({{ site.imageurl }}2009/Synology-DownloadStation2.png)

**Web Station**

This basically starts an Apache2 webserver, with the option of MySQL too - a LAMP stack in a small, good looking box!  PHP is enabled by default, and Synology host a [wiki of user confirmed compatible web applications](http://www.synology.com/wiki/index.php/User_Reported_Compatible_PHP/MySQL_Applications "PHP and MySQL Compatible Applications - Synology"), such as [phpBB](http://www.phpbb.com/ "phpBB - Bulletin Board"), [Joomla](http://www.joomla.org/ "Joomla - Content Management System"), [Drupal](http://drupal.org/ "Drupal - Content Management System"), [Piwigo](http://piwigo.org/ "Piwigo - Web Photo Album"), [SugarCRM](http://www.sugarcrm.com/crm/ "SugarCRM - Contact Relationship Managment"), oh, and DIY blogging tool called [WordPress](http://www.wordpress.org/ "Wordpress - Blogging Software") :wink:

I've got [DokuWiki](http://wiki.splitbrain.org/wiki:dokuwiki "DokuWiki - Wiki software") running at the moment as the user comments said [MediaWiki](http://www.mediawiki.org/ "MediaWiki - Wiki software") (which runs [Wikipedia](http://www.wikipedia.org/wiki "Wikipedia")) is a bit slow on the DS-107. Backups are easier with DokuWiki too as it's file based.

**Terminal - Telnet and SSH, getting under the hood**

Reading a few blog posts on the earlier DS-106 model, it seems there were patches that granted telnet access. Since then it seems Synology have quite rightly decided to provide simple Telnet and SSH access:

![Synology Management - Terminal options page]({{ site.imageurl }}2009/Synology-DSMTerminalManagement.png)

This means I've got an (albeit lightweight) Linux server, always on, sitting on the network..

With some [trivial instructions](http://rob.runtothehills.org/archives/25 "Synology Bootstrapping and Subversion Server - Run to the Hills"), I installed the [Itsy Package Management System](http://en.wikipedia.org/wiki/Ipkg "ipkg - Wikipedia.org") (which is called 'bootstrapping'), and gives access to 1,172 applications, tools and libraries!

And after securing SSH and poking a hole to it in my router, I can log into my Synology DS-107 from pretty much anywhere and get access not only to the files, but to the web interface via the magic of [SSH Tunneling](http://en.wikipedia.org/wiki/SSH_tunneling#SSH_tunneling "SSH Tunneling - Wikipedia").

**Subversion**

Among those extra applications is Subversion - the open source version control system which was a snap to install, configure and then (using the web Disk Station Manager 2.0 options) configure backups of both the Subversion repository and DokuWiki every hour to an old USB stick I plugged in the back.

The only thing that took me a bit of time to figure out was how to get 'svnserve' to start automatically. Do [check out my comment](http://runtothehills.org/rob/?p=25#comment-39 "Automatically starting svnserve on the Synology DS-107 - Blog comment") on [Rob's Synology Subversion post](http://runtothehills.org/rob/?p=25 "Synology Subversion Server - Run to the Hills") if you'd like to know what I came up with  <i class="fa fa-smile-o"></i>

**In summary**

It's a fantastic peice of kit - full of features, well made, small, looks good, has a great Ajax-y web interface, and with a little tweaking there's enough of a Linux server in there to run pretty much anything.

Well done Synology  <i class="fa fa-smile-o"></i>

<i class="fa fa-star text-warning"></i> <i class="fa fa-star text-warning"></i> <i class="fa fa-star text-warning"></i> <i class="fa fa-star text-warning"></i> <i class="fa fa-star text-warning"></i>