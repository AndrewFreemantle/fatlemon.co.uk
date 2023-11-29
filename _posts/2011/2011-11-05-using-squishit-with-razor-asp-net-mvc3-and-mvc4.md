---
title: 'Using SquishIt with Razor – ASP.Net MVC3 and MVC4'
permalink: /2011/11/using-squishit-with-razor-asp-net-mvc3-and-mvc4/
tags:
  - ASP.Net-MVC
  - Razor
  - SquishIt
---
I'll assume you already know the [reasons why you should minify and combine your CSS and Javascript](http://developer.yahoo.com/performance/rules.html#minify "Yahoo! Web developer performance rules - Minify").

My favourite of the tools available for .Net is [SquishIt](http://github.com/jetheredge/SquishIt/downloads "SquishIt - Download from GitHub") by [Justin Etheredge](http://www.codethinked.com/about "About Justin Etheredge") - here's how I hook it up in my ASP.Net MVC4 (and MVC3) Razor projects..

  1. Get the bits, [either directly](http://github.com/jetheredge/SquishIt/downloads "SquishIt - Download from GitHub") or find it in NuGet
  2. Add a Project Reference to the `SquishIt.Framework.dll`
  3. Add SquishIt to the namespaces section in the `Views/Web.config`:

``` xml
<system.web.webPages.razor>
  <pages pageBaseType="System.Web.Mvc.WebViewPage">
    <namespaces>
      ..
      <add namespace="SquishIt.Framework" />
      ..
    </namespaces>
  </pages>
</system.web.webPages.razor>
```

<ol start="4">
  <li>
    Replace any `&lt;link /&gt;` and `&lt;script /&gt;` references with the following*:
  </li>
</ol>

{% highlight html linenos %}
<!DOCTYPE html>
<html>
  <head>

  ..

  @MvcHtmlString.Create(
    Bundle.Css()
      .Add("/Content/Styles.css")
      .Add("/Content/MoreStyles.css")
      .Render("/Cache/Styles-combined.css"))

  @RenderSection("css", false)

  @MvcHtmlString.Create(
    Bundle.JavaScript()
      .Add("/Scripts/SomeScript.js")
      .Add("/Scripts/SomeOtherScript.js")
      .Add("/Scripts/YetAnotherScript.js")
      .Render("/Cache/Scripts-combined.js"))

  @RenderSection("js", false)

  ..

  </head>
{% endhighlight %}

<p class="notice--info text--info">
  <i class="fa fa-fw fa-sticky-note"></i><strong>Note:</strong> I'm loading the `&lt;script /&gt;`'s in the `&lt;head /&gt;` here for brevity, but there's nothing stopping us moving these two JavaScript statements to the bottom of our page so they're just above the closing `&lt;/body&gt;` as is generally recommended
</p>

<ol start="5">
  <li>Finally, as you'll have noticed in lines 11 & 20, we're generating the squished files to a <code>/Cache</code> directory, so we need to create it and grant the IIS AppPool our site is running under create and modify permissions to it</li>
</ol>


As I'm usually working with Layout pages, I'm sure you'll have also noticed the `@RenderSection()`'s in the example (lines 13 & 22) - that let's me inject page-specific Squished CSS and/or Javascript as needed like so:

``` csharp
@section css {
  @MvcHtmlString.Create(
    Bundle.Css()
      .Add("/Content/PageSpecificStyle.css")
      .Render("/Cache/PageSpecificStyle-combined.css"))
}

@section js {
  @MvcHtmlString.Create(
    Bundle.JavaScript()
      .Add("/Scripts/PageSpecificScript.js")
      .Add("/Scripts/AnotherPageSpecificScript.js")
      .Render("/Cache/PageSpecificScript-combined.js"))
}
```

Thank you Justin 🙂
