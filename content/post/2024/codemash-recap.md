---
title: "CodeMash 2024 Recap"
date: 2024-01-14T23:00:00-05:00
tags: [tech, conferences, codemash]
---

Last week I made my return to [CodeMash](https://codemash.org) for the first time in twelve years. I had a great time - two days of great talks and chats with friends (and coworkers!) old and new. I do not plan to wait over a decade to attend again. 😉

Some general thoughts and a session recap...

### Accessibility

This was a mixed bag, but I'm not going to fault the organizers here, for a couple of reasons:

- It's a small, regional, volunteer-run conference with, in recent years, a heavy emphasis on cost control and keeping the tickets as affordable as possible so you can attend even if your company isn't paying.
- Good captioning costs money (see above) and takes time to get organized. I didn't know I was attending until mid-November, and had I put in a formal request for accessible sessions at that point that's probably not enough time to get it set up.
- The organizers _were_ thinking about it; I heard from multiple people I know who were speakers, or involved with speaker selection, that they'd asked presenters to use a captioning solution if possible (I have a [whole blog post]( {{< relref "/post/2023/captioning-tools-extravaganza" >}} ) covering tools for this, btw 😉).
  - Not all presenters did this (thank you to the ones who did, though!), and depending on what presentation software they were using and how they were presenting, it may not have been an option for them - for example, Apple's Keynote does not have an integrated captioning solution.

I'm not criticizing. It's a process, takes time to get there, and it has to be something the organizers care about. They do, and if any of them are reading this I'm happy to talk with you about how to make this better in 2025. This sort of thing is one of the reasons I'm involved with the CNCF's [Deaf and Hard of Hearing Working Group](https://contribute.cncf.io/about/deaf-and-hard-of-hearing/) - I'm hoping that eventually we'll have a list of guidelines and solutions that will make it a lot easier for conferences to figure all of this out.

At any rate, even though most speakers did not provide captioning, CodeMash was still the most accessible conference experience I've ever had. The app I use on my iPhone, [Live Transcribe]({{<relref "/post/2023/captioning-tools-extravaganza" >}}#live-transcribehttpsappsapplecomappid1471473738-not-the-google-one-), knocked it out of the park. Between the Wednesday evening lightning talks, chatting with friends after the talks, breakfast through social time on Thursday, and breakfast through leaving for home on Friday (about half an hour early due to weather), I'd estimate I ran through 15+ hours of captioning time. With one exception in a room that didn't seem to have good sound quality, Live Transcribe was _great_ without me even needing to use an external microphone to get direct audio from the speaker. At meal times in a ballroom or during hallway chats, Live Transcribe was incredibly resilient against background noise allowing me to follow along and participate in conversations. I had an absolute blast and I'm looking forward to doing this again.

I would've liked to do this at Stir Trek, but the downside with the Stir Trek venue (AMC Easton in Columbus) is that they don't have wireless there, and it's an utter black hole for cell signal.

### Sessions

I attended a bunch of great sessions over the two days of CodeMash - my notes are way too extensive to easily summarize, but I'll try to share a few takeaways from each.

#### Cameron Vetter - How to Ground LLMs to Minimize Hallucinations

I've been working on an internal AI project at work and hallucinations are a thing that we have to deal with working with large language models. Cameron had great suggestions on managing this - too much to get into, but through careful prompting and testing, retrieval-augmented generation strategies, and picking the right models, you can get your hallucination rate down to well under five percent.

I also appreciated his emphasis that you don't need to automatically pick the most expensive/most capable model for everything - figure out what's the _least capable_ model that accomplishes your task, and go from there. This is something I've advocated for a while, and it was nice to see someone else validate that. 😉

#### Erissa Duval - Reasonable Accommodations in the Workplace

I've had my own experiences in the past with requesting workplace accommodations, so I was curious to see what recommendations she would have in her talk - which she was presenting for the first time. She had a good overview of the regulations on when companies do and don't need to offer reasonable accommodations, and also a great statistic - 58% of reasonable accommodations _cost nothing_, so even if you're a small company that the Americans with Disabilities Act technically doesn't apply to, there is _no reason_ not to listen when one of your employees asks for reasonable accommodations.

Two other great points - first, even if you have a very flexible boss who has no problem making those accommodations, it's worth going through the process with HR to make sure that you have documentation of your needs in case you ever change managers. And second, if someone is requesting a tech-based accommodation, it's important to get IT involved as soon as possible to make sure that HR doesn't make unrealistic promises.

#### Arthur Doler - Handling the Dark Forest: Survival When the World's On Fire

This was one of my favorite talks from the conference - Arthur gave a great presentation on brains, psychology, dealing with stress and trauma, and how the past few years impacted everyone. A humorous talk and his (hand drawn!) slides with the stinger text were great. If you get a chance to see this one in person I really recommend it, and if not, he also [has it posted on YouTube](https://www.youtube.com/watch?v=4wl37K27ohM).

#### Jeremy Miller - A Contrarian View on Software Architecture

This is a talk I was looking forward to. I've followed Jeremy's blog for years and the work he's doing with the "Critter stack" is really interesting. He is not a big fan of many of the prescriptive approaches to software architecture such as clean architecture/hexagonal architecture - they come from a place of well-meaning intentions, but they're inflexible and tend to go off the rails. Instead, he looks for ways to keep related bits of code together and avoid as much high-ceremony work.

Two points that resonated with me: first, that one of the reasons we are in the trough of disillusionment with microservices is that we are still bringing high-ceremony approaches to them that (in .NET) use a ton of projects in the solution, which _defeats the purpose_ of microservices in the first place. Instead, he advocates writing low-ceremony code, which you can get away with because it (should be) a smaller piece of the system to begin with. Second, your choice of data store really impacts how easy testing is; it's hard to do real unit/integration testing against a relational database since clearing all the tables gets painful, but it's _very easy_ to do this with a document-focused database such as the way Marten works with Postgres; you can simply run your test and easily nuke the database afterward.

#### Cory House - Out of Control: Rescuing a Failing Project

I think this was probably my favorite session of the whole conference. Cory's an excellent speaker, I've enjoyed watching his talks, and this was no exception. Cory listed out his 15 principles for rescuing struggling projects and in some ways I felt like he'd summed up the past few years of my career. I won't list out all of his principles here, but the ones I most agreed with:

- If it's worth checking, it's worth automating (i.e., code formatting, enforcing standards, "don't use this library", spelling, etc.)
- Use a checklist if it can't be automated
- Document decisions (colocate docs with your code, have a README, use ADRs, code comments should focus on _why_)
- Run code in your code reviews (don't just look at it in GitHub's PR interface and say "looks good, ship it", auto-deploy feature branches)

All-around great talk with fantastic advice. This one was in the main ballroom, so I think it was recorded and will be posted eventually.

#### Tori Brenneison - The Vue.js Power Hour

Started off my Friday morning with a great talk by Tori building a basic Vue 3 app in the span of an hour. She tempted the live demo gods and though it did not go perfectly smoothly, she recovered well, and it was a good "overVue"; I picked up some useful tips. I've done a few live demos myself, and it's always a little nerve-wracking. The only downside was that my brain was not quite ready for an 8:30am technical talk, despite my taking a "caffeinate early and often" approach to Friday...

#### Steve Smith - Clean Architecture with ASP.NET

I was interested to see this, having worked in a company that had bought into the Clean Architecture approach and having attended Jeremy's talk the day before. I don't know that there's a specific thing that jumped out at me, though I do agree with his view that we should push people into the pit of success by making it easy to do the right thing and hard to do the wrong thing. I think, if anything, it reinforces that both Steve and Jeremy's architecture camps are coming from places of good intentions and wanting to build better software; they just disagree on the best way to get there.

#### Jeremy Miller - CQRS With Event Sourcing Using the "Critter Stack"

This was a nice overview of [the posts in Jeremy's "Building a Critter Stack Application" series](https://jeremydmiller.com/2023/11/28/building-a-critter-stack-application-event-storming/) complete with examples from the sample app. I didn't take a ton of notes on this but enjoyed seeing him talk through it and demonstrate how to use Wolverine. I also had a nice conversation with him afterward asking if he had any thoughts on where event sourcing would be appropriate since it's one of those technologies that always sounds useful, but I struggle to find a place where it would fit well as an approach. He pointed me to [event-driven.io](https://event-driven.io/en/) so I guess I have plenty of reading to do in my near future.

#### Marko Skugor - Micro Front-Ends

This was a great overview on micro front-ends (basically, microservices except for your front-end app) by a former colleague of mine. I am not as deeply in the front-end world as some so I must confess bits of it went over my head, but there were some good bits in it that I think can be applicable to microservices as a whole, too. For example, one of the antipatterns he listed is using micro-frontends so your teams can use different frameworks (it's bad for performance). I would similarly argue that using microservices so your teams can use different backend languages is a bad idea, because you're [needlessly spending innovation tokens](https://mcfunley.com/choose-boring-technology) when you do that.

#### Sean Wendig - You Are the Pilot: Getting Better Code Gen with AI

For my last session of the conference - prior to a slightly early departure to get ahead of bad weather - I hit up Sean's session on generating code with ChatGPT and GitHub Copilot. This turned out to be a pretty basic introductory session and I don't think it covered anything I wasn't aware of, but that was totally fine - it was a fun and engaging presentation with Sean tempting the stochastic parrot demo gods by working through some problems in real time. The thing he emphasized - and this matches my own experience - is you should work through things incrementally. Don't try to solve the whole problem in a single prompt (it's not going to end well), break it down and approach it in small steps like you would anyway. Oh, and don't outsource your thinking to it, because ultimately it's _your_ name that's going on the pull request.

### Wrapping Up

All around, it was a great experience being back at CodeMash, and having excellent accessibility was a real novelty for me. I'm already excited about next year!
