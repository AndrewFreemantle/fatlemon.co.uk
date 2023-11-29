---
title: SSD, Visual Studio and improving developer productivity
permalink: /2010/02/ssd-visual-studio-and-improving-developer-productivity/
tags:
  - Productivity
  - SSD
  - Visual Studio
---
After [reading](http://www.tomshardware.com/reviews/windows-ssd-performance,2518.html "Windows SSD Performance - www.tomshardware.com") [lots](http://www.exdream.com/Blog/post/2009/02/25/Why-cheap-SSD-sucks-for-Visual-Studio.aspx "Why cheap SSD sucks for Visual Studio - exdream.com") [of](href="http://www.overclockers.co.uk/showproduct.php?prodid=HD-004-CS&tool=3&groupid=1657&catid=1660&subcat=1668 "Corsair X64 SSD Customer Reviews - OverClockers.co.uk") [reviews](http://www.ebuyer.com/product/167203/show_product_reviews "Corsair P128 SSD Customer Reviews - www.eBuyer.com"), [articles](http://stackoverflow.com/questions/499889/ssd-and-programming "SSD and Programming question - StackOverflow.com") and watching a [few](http://www.youtube.com/watch?v=Dt6VbOY3xE0 "SSD vs 7200rpm HDD - YouTube.com") [youtube](http://www.youtube.com/watch?v=LCBfsfzHPeY "MacBook HDD vs SSD boot time - YouTube") [videos](http://www.youtube.com/watch?v=Pf_QS3mZsyU "HDD vs SSD, Windows Vista tests - YouTube.com") comparing Solid State Drives to Hard Disk Drives, I was unsure that'd see a beneficial improvement in my day-to-day developer tools of Visual Studio, such as building and spinning up desktop and web applications.

I was unsure because there is a [consensus of small writes to SSDs aren't as breathtaking](http://en.wikipedia.org/wiki/Solid-state_drive#Disadvantages "SSD Disadvantages - www.wikipedia.org") as the reads and writes of larger files:

> SATA-based SSDs generally exhibit much slower write speeds. As erase blocks on flash-based SSDs generally are quite large (e.g. 0.5 - 1 megabyte), they are far slower than conventional disks during small writes (*write amplification* effect) and can suffer from write fragmentation.
>
> \- Wikipedia

But these things are fast at everything else, and while mulling over the idea of upgrading for a few months, I started to notice my usage patterns of switching between lots of different supporting applications. **Given that'd I'd get improvements elsewhere if not in Visual Studio, I decided to try it out for myself.**

The SSD I chose is the Corsair CMFSSD-64D1, otherwise known as the [Corsair Extreme Series X64](http://www.amazon.co.uk/gp/product/B002HQ2JXG?ie=UTF8&tag=fatl-21&linkCode=as2&camp=1634&creative=19450&creativeASIN=B002HQ2JXG "Corsair Extreme Series X64 - Amazon.co.uk"):

![Corsair Extreme X64 Solid State Disk]({{ site.imageurl }}2010/CorsairExtremeX64SSD.png)

Here are the timings I took with the original Samsung 80GB 7,200rpm drive, and the timings from the [Corsair Extreme X64](http://www.amazon.co.uk/gp/product/B002HQ2JXG?ie=UTF8&tag=fatl-21&linkCode=as2&camp=1634&creative=19450&creativeASIN=B002HQ2JXG "Corsair Extreme Series X64 - Amazon.co.uk"):
(all timings are in seconds, so lower numbers are better)

<table class="table table-striped hdd-vs-ssd">
  <thead>
    <tr>
      <td></td>
      <td><span class="table-subheading">boot</span> from cold to login prompt</td>
      <td><span class="table-subheading">logging in</span> pressing enter to ready</td>
      <td><span class="table-subheading">starting outlook</span> to 'All folders up to date'</td>
      <td><span class="table-subheading">loading VS2008</span> to news feed loads</td>
      <td><span class="table-subheading">loading solution</span> 8 project web solution</td>
      <td><span class="table-subheading">rebuild solution</span> until build status says 'build complete'</td>
    </tr>
  </thead>
  <tbody>
    <tr class="hdd">
      <td class="nowrap">Samsung HDD</td>
      <td style="text-align:right">42.3</td>
      <td style="text-align:right">190.8</td>
      <td style="text-align:right">14.7</td>
      <td style="text-align:right">25.2</td>
      <td style="text-align:right">72.0</td>
      <td style="text-align:right">79.0</td>
    </tr>
    <tr class="ssd">
      <td class="nowrap">Corsair X64 SSD</td>
      <td style="text-align:right">32.6</td>
      <td style="text-align:right">75.8</td>
      <td style="text-align:right">5.5</td>
      <td style="text-align:right">7.9</td>
      <td style="text-align:right">19.1</td>
      <td style="text-align:right">52.5</td>
    </tr>
  </tbody>
  <tfoot style="font-weight:bold">
    <tr>
      <td></td>
      <td style="text-align:right">22.8%</td>
      <td style="text-align:right">60.3%</td>
      <td style="text-align:right">63.0%</td>
      <td style="text-align:right">68.4%</td>
      <td style="text-align:right">73.5%</td>
      <td style="text-align:right">33.5%</td>
    </tr>
  </tfoot>
</table>

## Conclusion

**Overall, the SSD performs *53.6%* faster - that means my machine is twice as responsive as it was.**

However, the most important savings to me are loading, and building solutions - I've only been running the SSD for a couple of days, but already I have shaken the reflexive pause I felt before closing or restarting Visual Studio, or opening up different solutions.

Granted, building or compiling decent sized projects in Visual Studio on an SSD is not as impressively fast as spinning up applications (Outlook, Paint.Net or Dreamweaver for example), or the improvement in general system responsiveness.

I now get 3 complete builds for 2..  **put simply, it's almost fast enough to remove the build waiting tax**. It's fast enough that [I don't lose my train of thought and context switch to something else](http://stackoverflow.com/questions/499889/ssd-and-programming/499959#499959 "&quot;long compile time leads to distraction&quot; - answer on StackOverflow.com") - which makes me more productive.

## Method

Timings were taken manually with a stop-watch, and averaged over 3 data points on different days.

* The Windows logging in timings include a start-up anti-virus scan.

System Specification and software versions:

  * Intel Pentium 4 HT, 3.0 GHz
  * 3 GB RAM
  * Windows XP 32bit (HDD timings), Windows 7 32bit (SSD timings)*
  * Outlook 2007
  * Visual Studio 2008 Team System Developer Edition
  * The 8 project web solution is the [fantastic invoice management software called SNAP](http://www.snapsystems.co.uk/ "Invoice Management Software - Scan Notify Approve and Pay (SNAP) - www.snapsystems.co.uk"), written in C# .Net 3.5

\* yes, I'd have loved to have done an apples-to-apples comparison by copying the exact same Windows XP installation over to the SSD, and I was all set to do so, except the Samsung HDD had a series of faults around the ~36GB mark that resulted in an XP installation that would only boot into Safe Mode.

I had taken the timings the week before I found this out, and with a clean SSD it seemed crazy not to take the opportunity to upgrade to Windows 7. I suspect that had I stayed on Windows XP, the speed improvements would have been more impressive.

Fortuitously, the Corsair SSD arrived the same morning that the Samsung HDD exhibited the fault. (I know what you're thinking, and no, it definitely wasn't sabotage.. the SSD was on order for 2 weeks prior, and wasn't due to arrive for another 3 days)

The Solid State Drive upgrade is a definite improvement, and I don't think I'll be buying Hard Disk Drives any more 🙂
