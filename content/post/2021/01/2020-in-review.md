---
title: "2020 in Review"
date: 2021-01-02
---

It's been quite a while since I blogged consistently. Since I would like to develop a habit of writing again this year and there's been a lot going on, it seems like a great time to drop a year-in-review post.

Some background that I haven't mentioned here: I was laid off from my old job in fall of 2019, then took a new contract-to-hire role much closer to home a few months later. Trading in the 45 minute commute to/from Akron for one that was only five minutes was a ridiculously good quality of life improvement. That role converted to full-time at the beginning of March 2020, right before we all started working from home thanks to COVID-19 shelter-in-place orders. I haven't seen my office since mid-March.

## Working Remote

2020 wasn't my first remote rodeo; most recently at my previous job I was remote about 60% of the time, and had gone through stints being 100% remote and (nearly) 100% on-site; it depended on the team and project. This is the first time I've been full-time remote on a team that was remote-only, and I've found it's a _stunningly effective_ way to work. My job satisfaction is up, I feel more productive, I can have lunch with my wife. Everyone on the team still cares about each other and we take time to make some small talk, but our meetings are shorter and more effective. As a bonus for me, everyone having their own microphone has been _fantastic_ for having consistently working captioning.

The only thing I would change would be taking a page from the [Basecamp](https://basecamp.com/) folks and embracing a fully asynchronous work environment, leaving us with fewer meetings. However, that would be a major cultural change and a very different working style. Basecamp, on the other hand, has the advantage of an employee base that self-selected to that style of working in the first place.

That's not to say working remote in 2020 was a panacea. As Scott Hanselman pointed out back in April, [Quarantine work is not Remote work](https://www.hanselman.com/blog/quarantine-work-is-not-remote-work) and I've struggled with that myself at times. Like Scott, I've felt a little closer to the edge of burnout than is usual for me, and not having the usual remote-but-not-actually-home options like a coffee shop or cafe didn't help.

Overall, I've realized that remote works well for me, I hope to continue this arrangement in the future, and will be curious if my company eventually announces policy changes around it. They've traditionally been hostile to remote work, but to their credit, they pivoted _very_ quickly in mid-March and actually do a better job of it than my last company did with a de-facto remote environment due to distributed teams.

## Technology

For the past 14 months, I've been focused primarily on devops and release engineering tasks. I did quite a bit of work figuring out how to get initial proof-of-concepts working for deploying our various applications using Azure DevOps pipelines, although a lot of that has been superseded by efforts from my team's new lead developer who came in a few months ago. It always sucks a little to throw out your existing work, but he's more experienced with Azure DevOps and I've learned a lot from him, and that's awesome. I've also been heavily involved in release management for the team since transitioning to full time, which is... generally rewarding and occasionally frustrating, as release tasks often tend to be. It's a big responsibility that I appreciate being trusted with, but the other side of that coin is it's very stressful if it goes wrong. We always have room to improve; I strongly agree with [Charity Majors](https://twitter.com/mipsytipsy/) on things like frequent small releases, feature flags, and observability/tracing tooling and I'm looking forward to introducing a bunch of that. Getting to work on developer productivity and process improvements is a really high leverage activity and I'm looking forward to doing more of it in 2021.

## Career Development

Counting from my first full-time post-college job, I've been a professional software developer for 20 years this year, and when I look at my career so far I can see two ten-year arcs. The first ten year arc: learning how to be a professional, learning how to work with others, improving my technical skills, developing better soft skills and humility. The second ten years: reaching a senior dev role, continuing to learn, reaching a point where I can mostly take a task and run independently with it.

I'm not sure exactly what the third ten year arc of my career is going to be - I'd like to reach lead developer and perhaps an architect role, and I'm sure I'm going to continue learning and improving my skills. Never stop learning.

I'm also going to be thinking about what's important to me in my job, whether my current one or my next one. **Again, that's not because I'm in a hurry to move on.**[^1] Rather, it's because of something I learned the hard way. I stayed in my previous job for nearly 13 years, and while I really liked my team and the people I worked with... by the time I got laid off I was _miserable_ and staying, in part, through inertia. One of the things that I've learned about myself since then is that it really helps to take time periodically to reflect a bit on where you are and what you're doing, and make sure it still aligns with what you want to be doing. You shouldn't overthink this, but I don't think it's healthy to ignore it, either.

## Family Stuff

My daughter is now 3 1/2 and a relentless bundle of energy. It's often exhausting, but it's cool watching her connecting the dots (metaphorically) and learning new things. She really is a little sponge. Our dog is nuts and is too much of a doofus for her own good. Dog day care is good for her and since it wears her out, it's also good for us.

My wife's ending 2020 and starting 2021 doing a Java bootcamp, so it looks like we'll be a two-programmer household. Also a bit of a house divided; though I did Java for about 2-3 years at my previous job, C# remains my bread-and-butter programming language.

## Around the House

Nothing terribly exciting, honestly. We had some plumbing issues that needed fixed early in the pandemic, and our roof replaced in mid-November. Being a homeowner is surprisingly expensive.

## Hobbies and Side Projects

I haven't made as much time for these as I perhaps should. I enjoy co-op video games and have thoroughly enjoyed both Borderlands 3 and Diablo 3 with a friend; aside from that I really didn't play a lot of games in 2020. I only really make time to do that on Friday and Saturday nights anymore, and since I spend a ton of time in my home office during the day I haven't always wanted to at night, so often I just take my MacBook and go somewhere else.

Similarly, I didn't make a lot of progress on personal programming projects; mostly just noodling around with small things I wanted to understand better. I have some projects in mind for this year that I hope to get around to. Foremost among them: completely revamping my website, specifically...

* Move to a new DNS provider
* ~~Move back to a static site generator, although not Hexo this time~~[^2]
* Migrate domain email to Fastmail
* Migrate the site to a static-appropriate hosting provider such as Vercel, Netlify, or possibly Azure Static Web Sites
* Build out my own design and templates for the site

I decided to move back to a static site generator before I published this post; it's now up and running with a very basic Hugo theme. I may move over to something else eventually, but Hugo's a very happy medium for the moment.

I'd also like to make time this year to really learn a front-end framework for my personal projects - although we use Angular at work, I'm leaning toward Vue (seems lighter-weight) or React (800 pound gorilla and the default). I'd also like to make time to thoroughly learn a language that's a bit outside the C#/Java/Javascript mainstream I typically work with - either a functional language like F# or Elixir, or perhaps getting into Go or Rust.

[^1]: Adding this disclaimer for any of my coworkers who read this. 🙂

[^2]: Hexo is good software, but the documentation is rough in places and most of the community around it is not English-speaking. This is _completely fine_ but as I don't read Chinese, it makes finding examples of how to do things more than a bit frustrating. I can actually mark this one as done since you're reading this on a static-generated site _right now_. Hugo at the time of writing, though I may move it to an entirely custom-built Eleventy, Gridsome, or Gatsby site later on once I build my own templates for this.
