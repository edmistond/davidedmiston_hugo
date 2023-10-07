---
title: "Captioning Tools Extravaganza!"
date: 2023-10-09
---

So, you've found yourself attending or speaking at a conference that's not able to provide better accessibility tools. How do you navigate this?

### But first, a disclaimer...

I can't write or talk extensively about automatic captioning tools without taking a moment to note that they're controversial. Many in the Deaf community regard them as "craptions," don't think they provide good accessibility, and prefer actual [human captioners](https://whitecoatcaptioning.com/) instead. Others don't like captions at all, and prefer to have sign language interpreters available. Still others use automatic captioning extensively, find it adequate to their needs, and for whatever reason, prefer to use it.

Personally, I fall into this latter camp; I'm not ASL-fluent, and I've used automatic captioning extensively since late 2018. In the beginning, it was rather rough and inaccurate, but in the intervening years they've improved rather dramatically. I love and respect the human captioners that I've worked with, but having captions be instantly available any time of day or night was a game-changer for me, and I've progressed more in my career in the last five years than I did in the previous 15, solely due to being able to fully participate.

Worth noting: the biggest advantage of human captioners is they tend to be more accurate. If they've worked extensively with clients in the tech industry, they're likely to be familiar with the technical vocabulary you're using. They also tend to handle accents much better than automatic tools do.

The biggest *downside*, in my experience, with human captioners, is there can be more of a delay between someone speaking and the captions showing up. With the best captioners I've worked with, it's only a couple of seconds - even so, automatic captions tend to be even faster. For a big presentation, this isn't quite as much of a concern; I find delay more frustrating when I'm in a meeting with a fast-moving conversation.

Overall, my personal opinion is that auto-captions work well for me, and that they're a reasonable middle of the road option if a conference doesn't have the budget to offer something better, or for some reason isn't able to organize something better. However, I always advocate listening to the feedback you get from your attendees and, to the greatest extent possible, providing what they've stated they prefer.

### General Thoughts

First, I'd like to call out my operating context here: I'm a Midwestern American white male, working on teams primarily composed of the same. This, unfortunately, is the Platonic ideal for automatic speech to text processing. Depending on your accent, or your teammates', you may have radically different experiences than mine.

Some tools are able to both transcribe audio **and** translate it to another language at the same time. However, as I've never used this capability, I'm unable to say how well it works.

It's also important to emphasize that these tools are not perfect, and sometimes misunderstand what you say - this is pretty common for me because I have a strong "deaf" accent and don't sound like most other people (funnily, I'm occasionally asked if I'm British, so I guess that's how my accent comes across to American ears). One reason I mention this is because many of these tools offer profanity filters, and I've occasionally had them think I was swearing when I wasn't. This is something I wanted to call out; from a speaker's standpoint, if you're not accustomed to this and aren't expecting it, or expecting the audience to react to it, it might be a little surprising.

> A conference I regularly attend - Stir Trek in Columbus, Ohio - runs a series of repeating announcements on the projector screens before each talk. Including a brief educational slide about automatic captions might be a _great_ idea here.

With all of that said, let's examine options across various platforms. I will note if options are free or paid, but since prices are always changing, I'm not going to make an effort to call those out specifically.

### As a Presenter, I want captioning, so that my talk is accessible

Generally, the usual advice for speaking applies. Speak as clearly as you can, avoid cross-talk if you're co-presenting with someone. If you're doing a Q&A session, it's helpful to repeat the question back to make sure it shows up in the captioning feed.

If you're able, turn on captions while you're doing practice runs for the talk. This should give you a sense of how accurate these tools are and how they sometimes misunderstand what you say. If your talk uses uncommon tech jargon, it's helpful to know how it may come up on screen.

#### Windows Live Captions
- **Free, requires Windows 11 22H2 or newer, works offline**

_My thoughts_: Quick and pretty accurate, deals well with accents. Can be a little glitchy with repeating fragments of a sentence that's not finalized. Includes a profanity filter. I use this heavily every single day on both my work and home machines, and it's tremendously useful. This requires downloading several hundred megabytes of supporting files, so be sure to set it up ahead of time.

#### PowerPoint Live Captions
- **Free, included in PowerPoint, cloud-based**

_My thoughts_: I've seen one or two presentations that used this, and it seems to work well. It uses a cloud-based service, so you will need a stable internet connection for it to work. Also, the captions may stop, or at least won't be visible, if you switch out of PowerPoint to do a live demo of something.

#### Google Slides Live Captions
- **Free, included in Google Slides**

_My thoughts_: I haven't used this one personally; one of the draft reviewers mentioned it. Google Meet's built-in captions are high quality in my experience, so I would expect these to be on a similar level. Experiment first.

#### Apple Live Captions
- **Free, requires macOS 13 Ventura or newer, requires Apple Silicon, works offline**
- **Also available on iOS 16 or newer, requires iPhone 11 or newer**
- **Also available on iPadOS 16 or newer, [see instructions for supported devices](https://support.apple.com/guide/ipad/get-live-captions-beta-ipad0bbca12e/ipados)**
- **Does not work if screen recording (including DisplayLink) is active**

_My thoughts_: I appreciate that Apple added this. Unfortunately, I think it is the weakest of the available options. In my experience, it struggles more with accuracy and with technical vocabulary than other options do, but it's absolutely work trying it out for yourself. Also, note that depending on how your machine is set up, it may not function - Apple disables it by rendering a gray box over the captions window if any sort of screen recording software is active. This, unfortunately, includes when you are using DisplayLink adapters to get multiple external displays on M1/M2 Macs that wouldn't otherwise support it; DisplayLink on the Mac is implemented using screen recording.

#### Live Captions/April ASR
- **Free, Linux, macOS ARM and Intel, may require you to build it yourself, works offline**

Available [from GitHub](https://github.com/abb128/LiveCaptions) and [on FlatHub](https://flathub.org/apps/net.sapples.LiveCaptions).

*My thoughts*: I've been experimenting with this recently on my Mac, and it's remarkably good. In terms of captioning quality, I'd rank it just below Microsoft's own live captioning and well above Apple's. I haven't used it extensively yet but so far I'm really impressed with it. One disadvantage is that the window doesn't allow for resizing; two lines of captioning is all you get. I hacked a custom version to double that, but it's not an ideal solution.

If you want to use it on the Mac, the build process isn't onerous but is somewhat involved. I should document this in a future post.

### As a Conference, we want to offer captions, so everyone is welcome

Depending on your AV setup and how you are streaming data, you have a few options. You can of course hire an [awesome human captioning firm](https://whitecoatcaptioning.com/) to do this, but maybe you don't have that in your budget.

Probably the easiest approach in that case, depending on the conference AV setup, would be using Windows live captioning or the Linux/macOS Live Captions tools linked above. Another option might be to get in touch with Ava.me; [they've offered accessibility for conferences in the past](https://help.ava.me/en/articles/3160170-use-ava-at-conferences-venues-events-participant-info).

### As an Attendee, I want captions, so I know what the heck is going on

As a general rule, _all of the above_ options work great for attendees. If it's a virtual conference, you can use the built-in auto-captioning that many platforms now offer, or turn on one of the previously recommended tools. There are one or two others you can use, and then if you're in-person, we'll talk about actually getting sound _into_ your phone, laptop, or tablet in the first place.

#### [Otter.ai](https://otter.ai)
- **Free/paid, Android/iOS/iPadOS/web, cloud-based**

*My thoughts*: I used Otter extensively from 2019-2021, prior to Teams captioning improving and prior to Windows/Mac having live captioning options. I generally had very good luck with it, and ran a 3.5mm splitter off my laptop; this let me feed the audio into my iPad and use the app there, which was convenient for having my captions positioned right underneath the screen I was working on. Otter does require internet access and also saves the audio recording until you delete it; please be aware of any potential legal issues in the jurisdiction you're in if you use this. Additionally, Otter's license terms don't seem allow "public performance" so if anyone is thinking about using this from a conference or attendee standpoint... I don't think that's allowed.

#### [Ava.me](https://ava.me)
- **Free/paid, Android/iOS/iPadOS/Mac app, cloud-based**

*My thoughts*: This is the first captioning app I used, and it was a game changer for me. Unfortunately, it's not one of the more accurate ones unless you use the time-limited (per month) higher-accuracy captions. I love that they were a pioneer in this niche, but the other options in my experience are better now.

#### Google Live Transcribe
- **Free, Android, cloud based or on-device**

*My thoughts*: I'm not an Android user and don't regularly use this, but I have friends/family who have Android, and I've tested played with it. Live Transcribe and Google's on-device captions in general make me seriously contemplate switching to Android every time I'm ready for a phone upgrade. It's fast, it's good, if you have a Pixel or certain other Android phones it's _entirely on-device_. Mind blown. If you have a recent Android or Pixel phone, this is probably going to be your best bet.

#### [Live Transcribe](https://apps.apple.com/app/id1471473738) (NOT the Google one 🙂)
- **Paid (monthly subscription + additional hours IAP), iOS/iPadOS, cloud-based**

*My thoughts*: I'm an iPhone user and this is my go-to app for the iPhone and iPad. Very accurate when using the "premium" captions (rather than basic on-device which is just the built-in iOS STT). $5 a month subscription gives you five hours of premium captions, additional 1 or 5 hour blocks are available as in-app purchases for $1 an hour as of this writing. All around great and life-changing for me.

#### [CAPTION.Ninja](https://caption.ninja/) in Google Chrome
- **Free, Google Chrome, cloud-based**

*My thoughts*: Uses the cloud-based speech to text API in Google Chrome, so it does require an internet connection. Not as good as some of Google's other offerings, but nice in a pinch.

### As an Attendee, I want to get audio into my devices, so that I can have captions

So... you've evaluated the options above, picked a few, and now need to figure out how to get the audio into your device. You *could* just rock up to the talk you want to attend, hold your phone up, and hope for the best... but there are better ways.

The absolute best option would be if you can get an audio feed directly from the conference AV team into your device through the 3.5mm headset jack, but wiring and conference room setup can make this challenging. Another great option, if it's available, would be some kind of theater wireless system (if they have it) which can output to that 3.5mm headset jack.

Another option that works well, if the conference organizers are flexible and will give the speakers a heads-up, is to supply your own. In some ways, this isn't ideal; why should we have to pay our own money just for accessibility? I totally get it... but also, my personal philosophy is one of pragmatism. I like having a fallback option that doesn't depend on other people. Also, the same equipment I've used to provide my own accessibility at conferences is also useful in personal situations, like large family gatherings.

Overall, my favorite option for this is the [Hollyland Lark M1](https://www.hollyland.com/product/lark-m1) - it is a **dual** microphone system with good noise-canceling, and you can use both microphones simultaneously. The base receiver unit connects to your phone, tablet, or laptop via a 3.5mm TRS to TRRS cable; for many mobile devices you'll also need to provide your own Lightning or USB-C to 3.5mm headset adapter. If possible, I recommend using official Apple (or Google, though I can't vouch for them personally) adapters - I've tried one or two off Amazon that were marginally cheaper than the $10 or so that Apple and Google charge for theirs, and they didn't work. Now I just have two of the Apple adapters (Lightning and USB-C) and keep them in the fabric hard shell storage case the Lark comes in.

This was the tool I used at the most recent Stir Trek I attended - I had let the conf organizers know I needed accessibility support, they let the speakers know I might be stopping by. For each talk I'd just stop off quickly at the speaker's podium and ask them to wear one of the Lark's microphones (the dual microphones + charging case was helpful here - I always had one that was ready to go), then go find a seat and set up my captioning tool on whichever device I was using. It was the most access and best experience, by far, that I've ever had at a conference.

### Wrapping Up

This blog post has been brewing for quite a while; I don't think anyone has written up a comprehensive overview of captioning solutions. I've been involved with the CNCF Deaf/HOH Working Group and that's inspired me to finally get around to writing this; I hope that readers have found it useful. It's absolutely not ideal that we often end up doing the work for our own accessibility, but even so - it's useful to have some solutions you can recommend, and hopefully it's a good resource for conferences as well.

If you have any questions or feel I've missed an important option, please feel free to hit me up on Mastodon, email me (it's my last name and first initial, no punctuation, @ Google's mailing service :)), or find me on the CNCF Slack in #deaf-and-hard-of-hearing.

**Special Thanks**
I'd like to thank the following folks for reviewing this before publication: Catherine Paganini, AmyJune Hineline, Rob Koch

**AI Disclaimer:**
None of the text in this post was written by AI tools. I do sometimes use them for rubber-ducking and a sounding board, and [claude.ai](https://claude.ai/) did suggest a few points to consider elaborating on when I ran a late draft of this post through it.
