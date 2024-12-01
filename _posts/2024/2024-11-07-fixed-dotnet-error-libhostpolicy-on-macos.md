---
title: "Fixed: dotnet - An error occurred while loading required library libhostpolicy.dylib on macOS"
permalink: /2024/11/fixed-dotnet-an-error-occurred-while-loading-required-library-libhostpolicy.dylib-on-macos/
excerpt: "Manually installing the .Net SDK on macOS can be quarantined by Apple's security protections, thankfully it's a one-liner fix"
tags:
  - .net
  - dotnet
  - macOS
  - libhostpolicy.dylib
---

![]({{ site.imageurl }}2024/libhostpolicy/image.png)
<figcaption></figcaption>

Having [manually upgraded the .net SDK](https://learn.microsoft.com/en-gb/dotnet/core/install/macos?WT.mc_id=dotnet-35129-website#install-net-manually) to the latest sercurity patch (8.0.10 at the time) by downloading `dotnet-sdk-8.0.403-osx-arm64.tar.gz`, verifying the SHA (with `shasum -a 512 {file.tar.gz}`) and installing it with:

``` shell
tar -xf ~/Downloads/dotnet-sdk-8.0.403-osx-arm64.tar.gz -C $DOTNET_HOME

dotnet --version
```

I was then presented with the following error:

> Failed to load /Users/andrew/.dotnet/shared/Microsoft.NETCore.App/8.0.10/libhostpolicy.dylib, error: dlopen(/Users/andrew/.dotnet/shared/Microsoft.NETCore.App/8.0.10/libhostpolicy.dylib, 0x0001): tried: '/Users/andrew/.dotnet/shared/Microsoft.NETCore.App/8.0.10/libhostpolicy.dylib' (code signature in <01304A40-8F81-33A7-A0B6-8A333DC79FC2> '/Users/andrew/.dotnet/shared/Microsoft.NETCore.App/8.0.10/libhostpolicy.dylib' not valid for use in process: library load disallowed by system policy), '/System/Volumes/Preboot/Cryptexes/OS/Users/andrew/.dotnet/shared/Microsoft.NETCore.App/8.0.10/libhostpolicy.dylib' (no such file), '/Users/andrew/.dotnet/shared/Microsoft.NETCore.App/8.0.10/libhostpolicy.dylib' (code signature in <01304A40-8F81-33A7-A0B6-8A333DC79FC2> '/Users/andrew/.dotnet/shared/Microsoft.NETCore.App/8.0.10/libhostpolicy.dylib' not valid for use in process: library load disallowed by system policy)
> An error occurred while loading required library libhostpolicy.dylib from [/Users/andrew/.dotnet/shared/Microsoft.NETCore.App/8.0.10]

The key part of this error message is: <em>library load disallowed by system policy</em> - turns out that the protections in macOS mean, ironically, this security patch is flagged as insecure and quarantined.

[The fix is simple](https://github.com/dotnet/runtime/issues/95259#issuecomment-1828007858):

``` shell
cd $DOTNET_HOME

xattr -d -r com.apple.quarantine .
```

![]({{ site.imageurl }}2024/libhostpolicy/image-fixed.png)