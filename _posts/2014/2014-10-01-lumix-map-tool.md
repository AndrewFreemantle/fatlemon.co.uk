---
title: 'Panasonic LUMIX MapTool.pkg – Open Source edition'
author: Andrew Freemantle
layout: post
permalink: /2014/10/lumix-map-tool/
redirect_from:
  - /lumix-map-tool/
tags:
  - DMC-TZ30
  - DMC-TZ40
  - Lumix-Map-Tool
  - Open Source
---
![The Panasonic LUMIX DMC-TZ40 Digital Camera]({{ site.imageurl }}2014/Panasonic-DMC-TZ40.jpg)
<p class="wp-caption-text">The Panasonic LUMIX DMC-TZ40 Digital Camera, in a word, excellent!</p>

I recently upgraded my compact travel zoom camera from the tried and trusted Sony DMC-HX9V to the Panasonic DMC-TZ40. It's quite an upgrade and while I'm delighted by my new purchase, the review will have to wait now that I can use a major feature: MAPS!
  
![Maps! On a Digital Camera! What ever will they think of next!]({{ site.imageurl }}2014/Panasonic-LUMIX-TZ-Maps.jpg)
<p class="wp-caption-text">Maps! On a Digital Camera! What ever will they think of next!</p>


Yes, as well as GPS Geotagging of photographs, the Panasonic LUMIX DMC-TZ30 (ZS20) and Panasonic LUMIX DMC-TZ40 (LZ30) come with Map Data that you can use to find your way to your next photo shooting location, or back to your hostel if you find yourself lost!

The Map Data comes on the CD-ROM / DVD with the camera, along with a little application called the LUMIX Map Tool. There's a version for Microsoft Windows and Apple Mac OSX, but not for Linux.

To save on space I'd copied the contents of the DVD onto a USB drive so I could update the map data while travelling, but the only thing that didn't copy was the Apple Mac OSX version of the LUMIX MapTool.pkg!

It took a bit of Googling, but I eventually found a great [blog post by Roland Kluge where he'd written a simple script version of the tool for his LUMIX DMC-TZ31](http://blog.roland-kluge.de/?p=250 "Simple replacement for Lumix Map Tool (Python) - Roland Kluge") - and after reading the comments I was able to modify his script to make it work for my new LUMIX DMC-TZ40.

I cannot stress how thankful I am for his work and the comments on his post - **thank you Roland, and [thank you commenter Falk][2]**

### Introducing LUMIX Map Tool - Open Source edition :o)
  
![My contribution to Roland Kluge's Simple Replacement for Lumix Map Tool]({{ site.imageurl }}/2014/Lumix-Map-Tool.png)
  
<p class="wp-caption-text">My contribution to Roland Kluge's Simple Replacement for Lumix Map Tool</p>


[I've forked Roland's code](https://github.com/AndrewFreemantle/Lumix-Map-Tool "Limux-Map-Tool - source code on GitHub") and added support for the Lumix DMC-TZ40, and I thought I'd make it a little more interactive as I'm not likely to change the Map Data too often.

Simply download the `maptool.py` file [from the GitHub repository](https://github.com/AndrewFreemantle/Lumix-Map-Tool "Limux-Map-Tool - source code on GitHub") and run it with..

{% highlight bash %}
$ python maptool.py
{% endhighlight %}

.. and it will prompt you for the information it needs to get the Map Data from your DVD onto a formatted SD Card - *maps away!*
