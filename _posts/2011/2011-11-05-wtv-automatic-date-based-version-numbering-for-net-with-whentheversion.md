---
title: 'WTV: Automatic date-based version numbering for .Net with WhenTheVersion'
author: Andrew Freemantle
layout: post
permalink: /2011/11/wtv-automatic-date-based-version-numbering-for-net-with-whentheversion/
redirect_from:
  - /wtv/
tags:
  - .net
  - Development
  - SubVersion
  - Visual Studio
  - WTV
---
There's a trend towards using the software build date as part of the software version number, you know the sort of thing I mean: '<span class="inline-code">2011.11.4.xyz</span>', like in the footer over at [StackOverflow.com](http://www.stackoverflow.com "StackOverflow.com - Q&A for Programmers"), or [MyLetts.com](http://www.myletts.com "MyLetts.com - Lettings management for private landlords")..

[![WTV Date-version example: MyLetts.com]({{ site.imageurl }}2011/WTV-Date-Version-Example-MyLetts.png)
](http://www.myletts.com "MyLetts.com - Lettings management for private landlords")

Wouldn't it be great if we could get Visual Studio to do this for us automatically on build?  I thought so too, so I've written a little app to do it :smiley:

#### Getting set up with "When The Version"

  1. Go get a copy of "When The Version" `WTV.exe` from [the GitHub download page](https://github.com/AndrewFreemantle/When-The-Version/downloads "When The Version - Download page at GitHub") (or you can [build it from the source](https://github.com/AndrewFreemantle/When-The-Version "When The Version - Source on GitHub") if you prefer)
  2. Drop it somewhere on your PC
  3. Now, in your Visual Studio project, expand the Properties and duplicate the `AssemblyInfo.cs / .vb` file - I rename the duplicate to `AssemblyInfo.Template.cs`:

![AssemblyInfo.Template.cs - Solution Explorer]({{ site.imageurl }}2011/WTV-Date-Version-Solution-Explorer.png)

<ol start="4">
  <li>
    Edit the <code>AssemblyInfo.Template.cs</code> file, putting in the placeholders for the date parts like so:
  </li>
</ol>

{% highlight csharp %}

// Version information for an assembly consists of the following four values:  
//  
//      Major Version  
//      Minor Version  
//      Build Number  
//      Revision  
//  
// You can specify all the values or you can default the Revision and Build Numbers  
// by using the '*' as shown below:  
// [assembly: AssemblyVersion("1.0.*")]  
[assembly: AssemblyVersion("{YYYY}.{MM}.{DD}.0")]  
[assembly: AssemblyFileVersion("{YYYY}.{MM}.{DD}.0")]

{% endhighlight %}

The following values are currently supported:

  * <span class="inline-code">{YYYY}</span> - the year
  * <span class="inline-code">{MM}</span> - the month
  * <span class="inline-code">{DD}</span> - the day
  * <span class="inline-code">{SVN}</span> - SubVersion revision number (more on this a bit later)

<ol start="5">
  <li>
    Now, open up the <code>Project Properties > Build Events</code>, and add the following Pre-build event command line (on a single line):
  </li>
</ol>

{% highlight bash %}

"C:\Path\To\wtv.exe"  
"$(ProjectDir)Properties\AssemblyInfo.Template.cs"  
"$(ProjectDir)Properties\AssemblyInfo.cs"

{% endhighlight %}

<ol start="6">
  <li>
    Finally, set the Build Action for the new <code>AssemblyInfo.Template.cs</code> file to 'None' and Build!
  </li>
</ol>


If you get stuck, pop open a Command Prompt and run `WTV.exe` without any parameters for usage instructions, or paste your full command line to see  any helpful error messages:

![WTV: Command prompt usage]({{ site.imageurl }}2011/WTV-Date-Version-Command-Prompt-Usage.png)


#### SubVersion support

If you're using SubVersion for your source control, `WTV` can also put the projects SVN revision number into the application version.

***There's only one caveat*** - .Net Application version number values are limited to a maximum of 32,767 (i.e. they're `Int16` / `short`). Therefore, if your SVN revision is higher <span class="inline-code">WTV</span> will only use the last 4 digits: e.g.: 32768 -> 2768.

To use the SVN revision number, simply add the `{SVN}` placeholder to your `AssemblyInfo.Template.cs / .vb` file like so:

{% highlight csharp %}

[assembly: AssemblyVersion("{YYYY}.{MM}.{DD}.{SVN}")]  
[assembly: AssemblyFileVersion("{YYYY}.{MM}.{DD}.{SVN}")]

{% endhighlight %}


.. and then add a couple of extra parameters to the Pre-build event command line so `WTV` knows where `SubWCRev.exe` is, something like this (again, on a single line):

{% highlight bash %}

"C:\Path\To\WTV.exe"  
"$(ProjectDir)Properties\AssemblyInfo.Template.cs"  
"$(ProjectDir)Properties\AssemblyInfo.cs"  
"C:\Program Files\TortoiseSVN\bin\SubWCRev.exe"  
"$(SolutionDir)."

{% endhighlight %}

And because the `AssemblyInfo.cs` is generated, you can remove it from source control (but don't remove it from your project!).


#### Finally

Comments and suggestions are welcome here or on the [projects issue tracker over at GitHub](https://github.com/AndrewFreemantle/When-The-Version/issues "WhenTheVersion - Issue tracker at GitHub"). I hope you find it useful :smiley:
