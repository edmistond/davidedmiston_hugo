---
title: "Minor Maui Aggravations in .NET 8 and VS Code"
date: 2024-01-19
---

Besides messing with Flutter on a coworkers' recommendation, I'm also making time to play with MAUI again now that dotnet 8 is out. Since Visual Studio for the Mac hits end-of-life on August 31, 2024, I figured it's a good time to test out the MAUI extension for Visual Studio Code.

Running through the manual dotnet 8 install process for MAUI was painless; I upgraded to the latest version of the SDK (8.0.101 as of this writing), made sure I had the .NET MAUI, C#, and C# Devkit extensions installed in VS Code, and made sure that I had an up to date XCode install via the app store. Then I dropped into iTerm and ran `dotnet workload install maui` (had to use sudo) and waited around while it pulled down packages.

Once that finished I used the Devkit in VS Code to create a new MAUI project, then jumped into the .csproj file to remove the references to Android. This is where the minor aggravation started, and I'm documenting it here in case anyone else hits this. Here's the key issue: I had format-on-save enabled in VS Code, and I have Redhat's XML extension installed. Csproj files are automatically recognized as XML and it autoformatted them on save, which meant that my `SupportOSPlatformVersion` blocks looked like this:

```xml
<SupportedOSPlatformVersion
    Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'maccatalyst'">17.2
    </SupportedOSPlatformVersion>
```

Turns out... this breaks the build, you'll get a lovely error that looks like this:

```
/usr/local/share/dotnet/packs/Microsoft.MacCatalyst.Sdk/17.2.8004/tools/
msbuild/iOS/Xamarin.Shared.targets(546,3): error : Could not map the Mac 
Catalyst version 17.2 [/Users/edmistond/projects/Maui8Testbed/Maui8Testbed/
Maui8Testbed.csproj::TargetFramework=net8.0-maccatalyst]

/usr/local/share/dotnet/packs/Microsoft.MacCatalyst.Sdk/17.2.8004/tools/
msbuild/iOS/Xamarin.Shared.targets(546,3): error : to a corresponding macOS
version. Valid Mac Catalyst versions are: 15.0, 13.3.1, 16.2, 14.3, 15.5, 
13.1, 16.0, 17.2, 14.1, 15.3, 16.5, 14.6, 13.4, 17.0, 16.3, 15.6, 13.2, 14.4, 
16.1, 14.2, 15.4, 16.6, 13.5, 14.7, 17.1, 15.2, 14.0, 16.4, 13.3, 14.5 
[/Users/edmistond/projects/Maui8Testbed/Maui8Testbed/Maui8Testbed.csproj::
TargetFramework=net8.0-maccatalyst]

Build FAILED.
```

_(I had to munge this error text a bit to fit it within my blog's text display width...)_

Fortunately, the solution is an easy fix and is obscurely documented in a [GitHub issue](https://github.com/xamarin/xamarin-macios/issues/19093). I turned off format-on-save and changed my XML node so that the `Condition` attribute, the value, and the closing brace all appear on the same line, and that got it working.

It never fails to amaze me how sensitive XML parsers are to little problems like that. At one job, I worked on an API that was a feature for feature (and bug-for-bug) reimplementation of an existing app, but white-labeling a third party supplier under the hood because we wanted out of that business. The API could return XML responses, and we had to make some weird contortions to force it to return the nodes in a specific order because it turned out the XML parser one of our customers used was sensitive to node ordering... and this was in 2018 or 2019, so we're not talking about the dark ages.
