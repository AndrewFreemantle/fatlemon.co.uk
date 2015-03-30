---
title: Gravatar HtmlHelper for ASP.Net MVC
author: Andrew Freemantle
layout: post
permalink: /2012/01/gravatar-htmlhelper-for-asp-net-mvc/
redirect_from:
  - /gravatar/
tags:
  - ASP.Net-MVC
  - Gravatar
---

<div class="alert alert-info"><strong>Update:</strong> This implementation is now linked from <a class="alert-link" href="http://en.gravatar.com/site/implement/images/libraries/" title="Gravatar.com - Libraries, Plugins, Applications, HOWTOs, etc">the official Gravatar website!</a>
</div>

Inspired by a recent project at work, here's a complete implementation of the [Gravatar](http://www.gravatar.com/ "Gravatar - A Globally Recognized Avatar") [image request API](http://en.gravatar.com/site/implement/images/ "Gravatar image request API documentation"), as an ASP.Net MVC 3 / MCV4 HtmlHelper.

It includes the help and documentation from the Gravatar API page, and automatically does HTTPS/SSL requests if included on a page that is served securely (secure requests can be forced too).

#### Getting Started

  1. Head over to the [project page on GitHub](https://github.com/AndrewFreemantle/Gravatar-HtmlHelper "Gravatar-HelpHelper - GitHub"), and save the single file <span class="inline-code">GravatarHtmlHelper.cs</span> into your project
  2. Start using it with the HtmlHelper sytax, like so:

{% highlight csharp %}
@Html.GravatarImage("user.name@email.com")
{% endhighlight %}

or

{% highlight csharp %}
@Html.GravatarImage(  
  "user.name@email.com",  
  32,  
  GravatarHtmlHelper.DefaultImage.Identicon,  
  false,  
  GravatarHtmlHelper.Rating.G)  
{% endhighlight %}


![Gravatars]({{ site.imageurl }}/2012/Gravatar-List.png)

and that's it! The helper method is progressively overloaded, so you need only supply the minimum parameters to get the Gravatar you're after.

Comments and suggestions are welcome here or on the [projects issue tracker over at GitHub](https://github.com/AndrewFreemantle/Gravatar-HtmlHelper/issues "Gravatar-HtmlHelper - Issue tracker at GitHub"). I hope you find it useful  :smiley:

 [1]: http://en.gravatar.com/site/implement/images/libraries/ "Gravatar.com - Libraries, Plugins, Applications, HOWTOs, etc"
 [2]: http://www.gravatar.com/ "Gravatar - A Globally Recognized Avatar"
 [3]: http://en.gravatar.com/site/implement/images/ "Gravatar image request API documentation"
 [4]: https://github.com/AndrewFreemantle/Gravatar-HtmlHelper "Gravatar-HelpHelper - GitHub"
 [5]: http://www.fatlemon.co.uk/wp-content/uploads/gravatar-list.png
 [6]: https://github.com/AndrewFreemantle/Gravatar-HtmlHelper/issues "Gravatar-HtmlHelper - Issue tracker at GitHub"
 [7]: http://www.fatlemon.co.uk/wp-includes/images/smilies/icon_smile.gif