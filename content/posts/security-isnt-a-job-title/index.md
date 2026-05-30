---
title: "Security Isn't a Job Title — It's a Way of Thinking"
date: 2026-05-26T00:00:00+01:00
draft: false
description: "Why security drew me in, and how I've already built it into how I live online — before I'm working in the field."
tags: ["security", "open-source", "privacy", "career", "personal"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

My interest in security did not start with a certification or a course. It started with two things happening at the same time — realising that companies were profiting from my data whether I liked it or not, and becoming a parent who needed to keep three kids safe online.

Those two things pushed me in the same direction. Understanding how technology actually works rather than just using it and hoping for the best.

## The Kids Problem

When you home educate your children the internet is part of the learning environment. That is mostly a good thing — the amount of genuinely excellent free education available online is remarkable. But it also means you cannot just hand a device to a child and walk away.

My first instinct was the obvious one. uBlock Origin on the browser, firewall rules on their laptops. Both helped. Neither was sufficient on its own. A determined child or a misdirected search can still end up somewhere you do not want, and browser level blocking does nothing if they are using a different browser or a different device entirely.

So I started learning about DNS properly. I had already seen it in my CompTIA studies — a name, a port number, a definition to memorise. On paper it was just another term. It did not click as something real until I was setting up a WordPress site for a family member's business and had to configure DNS records for their domain. Suddenly it was not abstract anymore. I could see what it was actually doing — translating names into addresses, routing traffic, sitting quietly underneath every request made on the internet. And once I understood it as a real thing rather than an exam topic I started thinking about how I could use it at home.

Self hosting a DNS resolver on the router was the proper solution but I did not have the setup or the time to do it properly at that point, and I did not want my attempts at tinkering to affect my wife's devices or my own.

What I found instead was NextDNS. Cloud based, configured per device, no router access needed. I could lock down the kids' devices with custom filtering profiles while leaving everything else untouched. The CLI client is open source under MIT licence — the backend service is not — and that is a fair trade off for a tool that solved the problem properly without requiring me to build out the infrastructure I did not yet have. A practical middle ground between doing nothing and going full self-hosted, and one I would recommend to anyone in a similar position.

## The Data Problem

Around the same time I was dealing with the kids' setup I started paying more attention to what I was giving away by using the tools I had always used. Gmail, Chrome, the default apps on every device — none of it was free in any meaningful sense. I was the product. My data was being harvested, profiled, and sold to advertisers in ways I had never thought to question because everyone around me was doing the same thing.

Once I saw it I could not unsee it. And I started making changes.

The first was email. I came across Proton Mail through recommendations online — a privacy focused email provider based in Switzerland, with end to end encryption as the default. What sealed it was the bundle. Proton offers mail, VPN, cloud storage, and a password manager under one subscription. I could replace Gmail and sort out a VPN at the same time. Two problems, one decision. I have not looked back.

Two extras have made the Proton ecosystem even stronger over time. SimpleLogin — an open source email alias service Proton acquired in 2022 — lets me create unique aliases on my own domain for every service I sign up for. If one of those services is breached the email exposed has no relationship to anything else I use. Proton Authenticator, launched in 2025 and fully open source under GPLv3, handles my two factor authentication codes across every device with end to end encrypted sync. It deepens the Proton dependency, but because it uses standard TOTP and supports full export, moving away is always an option.

For browsing I switched to Firefox, hardened with uBlock Origin, Multi-Account Containers for session isolation, and telemetry disabled. Dropping Chrome felt significant at the time. Now it just feels obvious.

## The One Thing I Would Recommend to Everyone

If I could get every person I know to do one thing it would be to use a password manager.

I found Bitwarden through the same rabbit hole that led me to Proton — online communities recommending open source, privacy respecting tools. I set it up expecting it to be useful. I did not expect it to be genuinely life changing in a mundane but real way.

The ability to have a strong, unique password for every account without having to remember any of them is something I cannot overstate. Autofill handles the friction. Bitwarden generates the passwords. I stopped reusing passwords entirely, which I had been doing for years like almost everyone else. The risk that comes with reused passwords — one breach exposing credentials across dozens of accounts — is something I had understood intellectually and ignored practically until I had a tool that made doing it properly effortless.

Bitwarden is open source. That matters for the same reason it matters across everything I use — the code is publicly auditable. Security researchers can and do inspect it. There is nowhere for anything dishonest to hide.

## Why Open Source Is Part of This

A thread runs through most of the tools I have mentioned — Bitwarden, LibreWolf, Signal, SimpleLogin, Proton Authenticator. Where I can choose open source I do, and the reason is not ideological. It is practical.

Open source software cannot quietly do things to you that it does not disclose. The code is there. Anyone can read it. That structural transparency is a security property in itself — it is much harder to build in hidden data collection or backdoors when the entire community can inspect your work. Proprietary software asks you to trust the company. Open source lets you verify.

That does not mean every privacy respecting tool has to be fully open source. NextDNS is the example I gave above — the client is open source, the backend is not, and I still use it because it solves a real problem well and operates honestly about what it is. The principle is to choose tools that have earned trust, with open source as a strong structural advantage when the option exists.

## Something for Everyone

I am aware that the setup I have described sounds like a lot. It was not built in a day and I am not suggesting anyone replicate all of it at once.

The point I want to make is that there is something at every level of technical enthusiasm. NextDNS works for someone who has never touched a router config. Proton Mail is as easy to use as Gmail. Bitwarden has browser extensions that make it invisible in daily use. You do not have to self host your own DNS resolver or run your own mail server to meaningfully improve your security posture. You can start as small as you want and go as deep as you want — the options exist across the whole spectrum.

Pick one thing. The password manager is where I would start. Everything else can follow at whatever pace makes sense for you.

## Why This Matters for the Career Direction

I am not yet working in security. Network+ is in progress, Security+ is next, and the homelab is where theory becomes practice. But the mindset came before any of that — shaped by trying to protect my family online and by paying attention to what was actually happening to my data.

That feels like the right order. Security is not something you bolt on at the end. It is a way of thinking about systems, defaults, and trust that changes how you approach everything. The job title comes later. The thinking starts now.
