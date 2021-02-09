---
layout: post
title: "Building a startup on 10 dollars a month"
date: 2021-01-15 17:10:48 -0800
categories: [startup, opinion]
tags: [startups, business, advice, cash-strapped, tech, opinion]
---


## Summary

When building [Fixed.link](https://fixed.link/) I made sure to be smart with money, and pay $10/month serving thousands of users. It is extremely important early on in your startup journey to be resourceful and make things work for you. How? Well, for SaaS startups specifically, use flexible tools (e.g. the managed K8/DB/load balancer/toaster service is nice, but costs far more than a VPS), “hack” things (use free tiers, competitors, to make your service run smooth), pick the smartest place for ads, get referrals for things, code — don't buy (unless you really need it) and that's what I got so far. Read more if you want ideas and examples of the above I've put into practice. Be forewarned, I make use of referrals of things I use that save money/get shit done, they're marked with *.

## The mindset going into founding a low-cost startup

It's likely the dream of every founder (or really, just anyone that exists) to be successful. But, it's no secret that only a fraction turn out to be successful. The realities of the startup game are harsh — even with early and adequate funds, there's no such thing as a sure thing. Now obviously, I'm not saying there is no correlation between success and funding (i.e. it doesn't hurt to be flush with cash), but for those who aren't so lucky, what can we do? Simply put, keep your expenses proportional to your comfort with risk. For me, being frugal is part of my DNA, and I knew the odds of overcoming early startup death were against me. Hence, I've strived to reduce any financial burdens (without unnecessary technical limitations) while still setting myself up to break through the market.

## Use flexible tools, know your scale

So we've covered the mindset going in to the startup world on a budget, but how do make something viable without breaking the bank? Well, the offering probably shouldn't be expensive to manufacture or provide. Succinctly, it should probably be software — and something scalable. Next, there's no point saving money if it impedes the ability to sell/off a product/service. That's where experience comes in. Know when to build rather than buy, know when to embrace the simple thing instead of fancy/new. Become an expert builder, a master tinkerer and a jack of all trades. Technology changes rapidly, SaaS becomes easy to use but expensive to keep. 

For example, microservice architecture is all the rave right now, and generally makes use of pay-per-instance services that offer easy to scale containers. It's pretty great when you're in need of ultra scalable, high traffic systems at the click of a button. But guess what? It's expensive! Know your scale, don't solve issues that aren't in the near future. A simple VPS (or collection of VPS) can offer a robust and cheap alternative. Personally, I use [Digital Oceans $10/month VPS](https://fixed.link/digitalOcean)* and have no issue serving thousands of requests daily. Looking to the future, using something like docker swarm makes scaling fairly easy. Some tools like CDNs, aggressive caching _everywhere_, queues and intelligent product planning can really make a night and day difference in respect to performance. It literally costs $0 dollars, and keeps RAM/CPU costs down.

## Embrace "Free" and work around costs

Make use of _free_ services, FOSS is great, but free is free. Fixed.link doesn't pay for emails, analytics/tracking services, or ads — well actually, other than a VPS, there are no extra expenses. I know right? You might be going "How can email be free?" - we send less than 300 a day, which is free at [SendInBlue](https://www.sendinblue.com/pricing/), when scale increases, they offer sweet deals to startups (just email them), meaning costs remain reasonable. I could have rolled my own email service, but from prior experience, this is where time > money, self-hosted email services are a pain. 

Many companies, even tiny little startups, believe they _need_ deep-mind, **AI** driven, growth-hacking analytics services to read the magic palms of the user. I can already hear them say "Without the black magic wizardry of expensive MarTech tools, how else can I determine the colour of my buttons?!". Having worked on that side of product, I've attended countless hours of meetings where a presentation walks through heatmaps of clicks on a page, and user journey flow charts through the catacombs of hell, and exclaims that *this means we must change/fix/break/add a new button/modal/input/colour*. Fixed.link uses [Umami](fixed.link/umamiProject), a FOSS analytics service. Super easy to self-host, super easy to use and far less invasive and privacy breaching than pay-to-pay services. It covers the basics, isn't powered by voodoo, and costs nothing. Ultimately, Find a tool that works for you but try not to buy more than you need.



