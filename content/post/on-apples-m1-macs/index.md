---
title: "On Apple's M1 Macs"
date: 2021-03-16
---

Compared to the first-day adopters, I'm a bit late to the party here... but as of the end of February I've joined the ranks of ARM MacBook users. After two weeks or so of sustained usage, I'm impressed.

The short version of how I ended up with it: I took my 2016 15" MBP in to the Apple Store (this was, I will note, a very interesting pandemic experience[^1]) to get the keyboard and "service battery" message evaluated. The laptop was too old to still qualify for the keyboard service program; replacing the battery for $200 would've resolved both problems, but they also offered me a pretty attractive trade-in value for a laptop with a bad keyboard and failing battery. They also had the configuration I wanted in stock for purchase that day, so I ended up downsizing to a 13" M1 MBP in the 16GB/1TB configuration.

Since my day job right now is .NET development, I did consider moving back to a Windows laptop, or to Linux; particularly as I hadn't been too enamored of some of the UI changes in Big Sur when I saw them. That said, I installed it a few months ago on my old MBP and after using it for a while, y'know what? It's _fine_. I wouldn't have made some of the UI choices they did, but it doesn't bother me day-to-day.

In no particular order, some impressions and comments on the machine after two weeks:

* Holy crap, it's _ridiculously fast_. The majority of apps open in one or two bounces on the Dock; essentially, it has the speed of an iPhone where you open, say, system prefs and it comes up _instantly_, except it's a Mac.
* I have never tended to prioritize battery life like some people do, but the battery life on the M1 MBP is bonkers. Lately I've been unplugging it fully charged, using it for an hour or three in the evening depending on what I'm doing, leaving it sleeping and unplugged overnight, then leaving it open and running for personal email and occasional messages while working the following morning. It's usually around 60% by lunch the next day. It's interesting how I just don't think about it now.
* Related, things that don't eat battery at an alarming rate anymore: FaceTime, Chromium-based browsers, JetBrains IDEs.
* The keyboard on this is a tremendous improvement over the 2016-19 era butterfly models, and is what they should've built in the first place. It's very pleasant to type on.
* Touch Bar remains fine. I didn't hate it on the 2016 and I don't hate it on this. Having a physical ESC key is an improvement.
* Rosetta 2 works amazingly well. I haven't had major issues with anything; Rider had some minor hiccups debugging .NET 5.0.103 applications, but I recently installed VS Mac which installed the 5.0.201 minor update and it no longer seems to be a problem. .NET 5.1 is supposed to fully support Rosetta, and .NET 6 this coming November should have an ARM-native version.
* Docker tech preview installs fine. I haven't had an opportunity to test it yet.
* Azul Zulu JDK 15 installs fine and is native.
* Homebrew is fine. I only installed the ARM version and so far that hasn't been a hassle.
* Chrome's live captions feature doesn't work on ARM Macs, which is annoying, but since it's still hidden behind an experimental flag I suppose I'll give them a pass for now. I really wish that Apple would do some kind of live captions across iOS/iPadOS/macOS and save me the hassles.
* Haven't really tried running iOS apps for the most part, with one exception related to the above: I can run [Otter.ai](https://otter.ai)'s iOS app to pull an audio stream from the microphone and auto-caption it. I can then use [Loopback](https://www.rogueamoeba.com/loopback/) to create a pass-through device, set it as my output device, and let Otter think _that_ is the microphone. In this way, I can get captions for video calls or videos that don't offer them. Again, I really wish Apple would just build this in.
* The webcam is still only 720p, but the software tweaks they've made noticeably improved it.
* Downgrading from a 15 to 13" screen has made one thing very clear: it's time for a new glasses prescription, and/or bigger fonts. _sigh_

Overall, I'm favorably impressed with what they've done here. The performance is terrific - it stacks up favorably against (or outperforms) relatively high-end Intel Macs and yet it's the _slowest_ ARM Mac they're ever going to release. I'll be very interested to see what they bring out in future hardware updates.

[^1]: That's an entire story on its own, I guess. It was fine, but... felt weird? Being inside an actual building with more than one or two other people felt very strange to me. COVID's entirely scrambled my brain.