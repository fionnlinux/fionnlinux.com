---
title: "Why Linux — and Why It Matters More Than You Think"
date: 2026-06-08T00:00:00-04:00
draft: false
description: "Linux has a reputation for being complicated. The reality is more interesting than that."
tags: ["linux", "open-source", "redhat", "career", "personal"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Linux has a reputation for being complicated. Something for developers and server administrators, not for ordinary people with ordinary computers. That reputation is not entirely undeserved — but it tells maybe ten percent of the story.

The other ninety percent is more interesting.

## Small Tools, Done Right

Anyone who has worked in a hands on environment — trades, manufacturing, warehousing — knows that good systems are built from components that each do their job correctly and efficiently. You think about how things connect, what each part is responsible for, and how a problem in one area affects everything downstream. That kind of thinking becomes second nature after a while.

Linux was built on a similar principle. The Unix philosophy — which underpins Linux — is that a program should do one thing and do it well, and that complex tasks should be handled by combining simple tools rather than building one bloated thing that tries to do everything at once. Small components, clearly defined, working together. For someone who has spent years thinking about systems that way, it is a comfortable way to think about software too.

It is worth being honest though — modern Linux distributions do not follow that philosophy as strictly as they once did. systemd, the init system used by most major distributions including Fedora and Red Hat, does a lot more than one thing. It manages services, logging, networking, timers, and more — all under one roof. Not everyone in the Linux community is happy about that. But that tension is part of what makes Linux interesting — it is a living ecosystem with real disagreements about the right way to do things, argued openly, in public, by the people who actually build it.

## It Was Already Everywhere

Before I started studying Linux seriously I thought of it as a niche choice — something enthusiasts ran on their personal machines or that lived quietly on web servers nobody thought about. The more I dug into it the more that assumption fell apart.

The majority of the world's web servers run Linux. Most cloud infrastructure runs Linux. Android — which runs on most smartphones — is built on the Linux kernel. Smart TVs, routers, industrial control systems — Linux turns up in places you would not expect once you start looking. I kept finding it in corners I had not anticipated and honestly it still surprises me how far it reaches.

The detail that caught me off guard most was Microsoft. Azure, Microsoft's own cloud platform, runs predominantly on Linux rather than Windows Server. The company that spent years selling Windows as the serious choice for business eventually built its cloud on the thing it once opposed. Make of that what you will — it says something about where the engineering led when the decision was purely practical.

Linux took over the internet and cloud infrastructure for fairly straightforward reasons from what I can gather. The licensing meant businesses could deploy it at scale without the costs that came with proprietary alternatives. The stability held up under serious load. And because the source code is open, problems tend to get found and fixed quickly by a large community of people who actually depend on it working.

The consumer desktop is a different story — Windows and macOS dominate there and that is unlikely to change any time soon. But the infrastructure underneath the internet, the servers quietly doing the work behind the applications everyone uses every day — that is largely Linux.

The point landed closest to home during a conversation at work. I have been shadowing the IT department at the manufacturing company I work for — trying to get some practical experience alongside my CompTIA studies. I got talking to the head of IT about my home setup and mentioned the projects I had been running with Podman, including a local LLM I had been experimenting with. She told me they were currently working through a proof of concept on the production floor using Red Hat Linux and Podman.

Linux was already running in the building I go to work in every day. I just had not known it until I started looking.

## Why Open Source Actually Matters

Free does not just mean no price tag. That is the part I did not fully appreciate until I started digging into what open source actually means in practice.

When software is open source the code is publicly available. Anyone can read it, audit it, modify it, and build on it. For Linux specifically that means if a distribution you rely on is abandoned or goes in a direction you disagree with, the community can fork it and carry it forward. It has happened before — Rocky Linux exists precisely because CentOS changed direction in a way the community did not accept. The project did not die. People picked it up and kept going.

That kind of resilience is not something you get with proprietary software. When a company decides to end a product, change its licensing, or pivot its business model, you are along for the ride whether you like it or not. With open source you have options.

The more I learned about how my data had been used over the years — through Microsoft, Google, Apple, the general ecosystem of products most people use without thinking — the more uncomfortable I became with those models. It was not one specific thing. It was the accumulation of it. Targeted advertising, data harvested and sold, features locked behind subscriptions, software that phones home constantly. I had accepted all of it without question for years because everyone else had too.

Linux and the open source ecosystem felt like the opposite of all that. Transparent by design. Community owned. Not built around extracting value from users.

There is also something that matters to me personally about access. Open source software runs on older hardware. It does not require expensive licences. It opens up access to genuinely good tools for people who could not otherwise afford them — and that connects directly to something I believe about education. Good resources should be available to everyone, not gated behind cost. The open source model, at its best, reflects that.

From a security perspective it surprises people sometimes to learn that some of the most capable tools in the industry are open source. Wazuh is a fully featured SIEM — the kind of security monitoring platform that would cost serious money in a proprietary form — available to anyone. OPNsense, which runs on FreeBSD rather than Linux but sits firmly in the open source Unix-like ecosystem, is a professional grade firewall that runs on modest hardware. The fact that I can build a homelab running real enterprise grade security tooling for the cost of a second hand server is still something I find remarkable.

The open source world is not perfect. Projects get abandoned. Documentation can be sparse. Support means searching forums rather than calling a helpdesk. But the trade-offs are worth it — and the community that forms around good open source projects is often more knowledgeable and more generous than any paid support contract I have come across.

## Why Red Hat

My introduction to the Red Hat ecosystem came through Fedora. Everywhere I looked online people were talking about it as the cutting edge of Linux — the latest kernel, the newest technologies, the distribution that pushed things forward before anyone else. That appealed to me. It also made me slightly nervous. Bleeding edge has a reputation for breaking things, and I was not sure it was the right place for someone still finding their feet.

What I did not fully appreciate at the time was the relationship between Fedora and Red Hat Enterprise Linux. Fedora is where new ideas are tested. Red Hat Enterprise Linux takes what works, stabilises it, and builds an enterprise grade product around it. Rocky Linux — which I use for my homelab — sits in that same ecosystem, a free community rebuild of RHEL. Three distributions, one lineage.

When I started looking into Linux certifications the obvious ones came up first — CompTIA Linux+, Linux Essentials, the kind of multiple choice exams I was already familiar with from the CompTIA pathway. Then RHCSA kept appearing, and the reason people gave for rating it above the others was consistent — it is a hands on exam. No multiple choice. You sit in front of a live system and you either know how to do the task or you do not. There is nowhere to hide.

That felt like the right way to be tested. Anyone can memorise answers. Actually doing the work is a different thing entirely and I respected that the certification reflected it.

The wider picture made the decision straightforward. Red Hat Enterprise Linux powers critical infrastructure across enterprise and government — including UK government departments, public health bodies, and critical national infrastructure on both sides of the Atlantic. It is not a niche choice or a hobbyist preference. It is what serious organisations run when reliability and support matter. Knowing that the skills I am building have direct relevance to that world, in both the UK and the US, made the direction clear.

Fedora on my daily driver. Rocky Linux in my homelab. RHCSA on the horizon. The ecosystem made sense before I fully understood why — and the more I learn the more that instinct gets confirmed.

## The Foundation Everything Else Is Built On

Linux is not just a technical choice. It is a statement about what technology should be and who it should serve.

The world is increasingly technology dependent. Laptops, internet access, capable software — these are not luxuries anymore. They are the infrastructure of modern life, the same way roads and electricity are. And yet access to good software still largely follows the same lines as access to everything else — those with resources get more, those without get less.

Linux pushes against that. Not perfectly, not always, and not without its own frustrations — but the direction is right. Free to use, free to modify, free to distribute. Running on hardware that proprietary systems have written off. Kept alive by communities of people who believe that good tools should be available to everyone.

Part of what makes it remarkable is the range of it. There are distributions built for people who just want a computer that works — something to browse the internet, watch videos, write documents, without ever touching a terminal. And there are distributions built for people who want to understand every layer of what their system is doing, configure everything from scratch, and learn how it all fits together. The same ecosystem serves both. You take from it what you need and leave the rest.

I did not come to Linux because I was a developer or a computer scientist. I came to it as someone who spent years in warehouses and on factory floors, who watched the pandemic expose how much freedom is tied to what kind of work you do, and who eventually decided that understanding the infrastructure the modern world runs on was worth the effort.

It has been worth the effort.

If you are sitting on the fence about Linux — curious but not sure it is for you — I would not tell you it is easy or that it will not occasionally frustrate you. I would tell you that it is honest. It does not hide what it is, it does not harvest what it does not need, and it does not lock you out of your own machine. In a world where most software is designed around extracting value from the people using it, that is rarer than it should be.

I would rather invest my time and energy in this ecosystem than hand it over to systems built behind paywalls and closed doors. And in whatever small way I can — through this site, through the things I build, through the things I share — I would rather be part of the movement trying to show people what Linux can be than sit quietly on the sidelines.

*Linux is the foundation. Everything else I am building is on top of it.
