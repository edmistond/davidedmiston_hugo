---
title: "Maui Setup"
date: 2023-03-07T21:25:30-05:00
tags: [dotnet, maui]
---

As noted in my [housekeeping post](/post/2023/03/housekeeping-notes/), I'm wanting to play with dotnet MAUI; a more desktop focused approach to coding would be an interesting paradigm shift, and I have ideas cooking for apps I'd like to build. However, MAUI is still new and in flux, and the process of getting an app to initially run and compile proved immensely frustrating, and I ran into the same issues on both my Windows desktop and my Mac over a few days of messing with it.

In both cases, I had the `dotnet` CLI templates for MAUI installed, and when I attempted to compile and run I would get "DEP0700" errors related to assets in the templates. The process of figuring out what was wrong here was rather frustrating, but I turned up a Github issue (which, unfortunately, I can't find the link to now) with the resolution: to create new projects, you need to **uninstall** the CLI templates ( `dotnet new uninstall microsoft.maui.templates` ) and then use the Visual Studio 2022 (Win or Mac) new project wizard to create the solution and project. On the Mac side, I ended up removing my entire dotnet, Mono, and VS 2022 toolchain and reinstalling entirely from scratch; on the Windows side, I only needed to remove the templates from the CLI tool.

Once I did that, I was able to create a test project on both platforms and run it against the Windows Desktop, Mac Catalyst, and iOS Emulator targets. Can't speak for Android, since I'm a long-time iOS user and haven't touched Android devices in some years.

The issue was a total pain to debug, but it sounds as though the CLI templates aren't fully up to date and the ones that VS references are more current. Once I got the projects created, I didn't have any problem opening the project in JetBrains Rider and running it against the Mac Catalyst target, though I'm not sure if it works with iOS. I'm more interested in it for desktop usage right now, so this is a (potential) limitation I'm fine with... as long as I can use Rider, which is my preferred IDE for most dotnet work now.
