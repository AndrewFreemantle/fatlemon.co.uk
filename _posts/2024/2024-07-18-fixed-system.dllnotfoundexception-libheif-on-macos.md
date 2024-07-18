---
title: "Fixed: System.DllNotFoundException: Unable to load shared library 'libheif' on macOS"
permalink: /2024/07/fixed-system.dllnotfoundexception-libheif-on-macos/
excerpt: "The NuGet package LibHeifSharp wraps libheif but on macOS libheif isn't where the .Net Framework expects it to be, but is quickly resolved with a symlink"
tags:
  - .net
  - macOS
  - DllNotFoundException
  - libheif
---

![]({{ site.imageurl }}2024/libheif/image.jpg)
<figcaption><a href="https://www.pexels.com/photo/pink-jigsaw-puzzle-piece-3482441/">Pink Jigsaw Puzzle Piece</a> photo by Ann H</figcaption>

Unlike installing executables that the operating system can find by trying each `PATH` entry configured in the environment, dynamically linked libraries have to exist in known locations.

Helpfully, the .Net Framework logs out those known locations before giving up and throwing the `System.DllNotFoundException`.

It's then a straightforward matter of locating the installed library and [symlinking](https://en.wikipedia.org/wiki/Symbolic_link) it into one of those known locations like so:

``` shell
ln -s /opt/homebrew/Cellar/libheif/1.17.6_1/lib/libheif.dylib $DOTNET_HOME/.dotnet/shared/Microsoft.NETCore.App/8.0.7/
```

_OK, but how do we know these locations?_

Firstly, here's an example abrieviated error log message:

```
fail: Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware[1]
      An unhandled exception has occurred while executing the request.
      System.DllNotFoundException: Unable to load shared library 'libheif' or one of its dependencies.
        tried: '{Project Directory}/bin/Debug/net8.0/runtimes/osx-arm64/native/libheif.dylib' (no such file)
        tried: '~/.dotnet/shared/Microsoft.NETCore.App/8.0.7/libheif.dylib' (no such file)
        tried: '{Project Directory}/bin/Debug/net8.0/libheif.dylib' (no such file)
        tried: 'libheif.dylib' (no such file),
          '/System/Volumes/Preboot/Cryptexes/OSlibheif.dylib' (no such file),
          '/usr/lib/libheif.dylib' (no such file, not in dyld cache),
          'libheif.dylib' (no such file),
          '/usr/local/lib/libheif.dylib' (no such file),
          '/usr/lib/libheif.dylib' (no such file, not in dyld cache)
        tried: '{Project Directory}/bin/Debug/net8.0/runtimes/osx-arm64/native/liblibheif.dylib' (no such file),
        tried: '~/.dotnet/shared/Microsoft.NETCore.App/8.0.7/liblibheif.dylib' (no such file),
        tried: '{Project Directory}/bin/Debug/net8.0/liblibheif.dylib' (no such file),
        tried: 'liblibheif.dylib' (no such file),
          '/System/Volumes/Preboot/Cryptexes/OSliblibheif.dylib' (no such file),
          '/usr/lib/liblibheif.dylib' (no such file, not in dyld cache),
          'liblibheif.dylib' (no such file),
          '/usr/local/lib/liblibheif.dylib' (no such file),
          '/usr/lib/liblibheif.dylib' (no such file, not in dyld cache)
        tried: '{Project Directory}/bin/Debug/net8.0/runtimes/osx-arm64/native/libheif' (no such file)
        tried: '~/.dotnet/shared/Microsoft.NETCore.App/8.0.7/libheif' (no such file),
        tried: '{Project Directory}/bin/Debug/net8.0/libheif' (no such file),
        tried: 'libheif' (no such file),
          '/System/Volumes/Preboot/Cryptexes/OSlibheif' (no such file),
          '/usr/lib/libheif' (no such file, not in dyld cache),
          'libheif' (no such file), '/usr/local/lib/libheif' (no such file),
          '/usr/lib/libheif' (no such file, not in dyld cache)
        tried: '{Project Directory}/bin/Debug/net8.0/runtimes/osx-arm64/native/liblibheif' (no such file)
        tried: '~/.dotnet/shared/Microsoft.NETCore.App/8.0.7/liblibheif' (no such file),
        tried: '{Project Directory}/bin/Debug/net8.0/liblibheif' (no such file),
        tried: 'liblibheif' (no such file),
          '/System/Volumes/Preboot/Cryptexes/OSliblibheif' (no such file),
          '/usr/lib/liblibheif' (no such file, not in dyld cache),
          'liblibheif' (no such file),
          '/usr/local/lib/liblibheif' (no such file),
          '/usr/lib/liblibheif' (no such file, not in dyld cache)
```

We can use the library filename it's looking for to either find the library by searching the entire OS (_which could take a while and so not recommended! `find / -name 'libheif.dylib'`_), or we could find out where we installed it manually - in this case it was installed via [homebrew](https://brew.sh):

``` shell
> brew info libheif
==> libheif: stable 1.17.6 (bottled)
ISO/IEC 23008-12:2017 HEIF file format decoder and encoder
https://www.libde265.org/
Installed
/opt/homebrew/Cellar/libheif/1.17.6_1 (30 files, 3.8MB) *
```

Then it's a case of constructing the `ln` command, having picked a suitable target location where .Net expects to find our installed library - I'd suggest a location under `DOTNET_HOME` like so:

``` shell
ln -s /opt/homebrew/Cellar/libheif/1.17.6_1/lib/libheif.dylib $DOTNET_HOME/shared/Microsoft.NETCore.App/8.0.7/
```

Note that the downside of this location is that updates to the .Net Framework will mean having to re-run this command for the new version.
