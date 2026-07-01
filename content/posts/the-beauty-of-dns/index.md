---
title: "The Beauty of DNS — Why One System Filters Everything You Connect To"
date: 2026-07-01T00:00:00+01:00
draft: false
description: "Record types, zones, recursion, and the security extensions layered on top — DNS explained the way I wish someone had explained it to me, grounded in choosing NextDNS over browser extensions and blocklists."
tags: ["networking", "dns", "comptia", "security"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Back at school in the mid-2000s, the internet was filtered, and we all knew it. Someone would find a proxy site that got past whatever the school had blocked, everyone would use it until that one got blocked too, and then someone would find another. None of us understood what was actually being filtered, or how, or why the proxy worked when going directly did not. It just worked until it stopped working, and then the search started again.

The beauty of DNS, once it actually sinks in, is realising what was almost certainly happening behind that wall the whole time. Almost everything a device connects to over the internet passes through DNS first. Every browser tab, every background service phoning home, every request for a website by name — before any of it can reach a server, it has to ask DNS where that name actually lives. Block or redirect that lookup, and you have blocked the site everywhere at once on that network, without touching the site itself. That is very likely the exact mechanism sitting behind a school's content filter, and it is also very likely why a proxy got round it — a proxy does its own DNS lookup somewhere else entirely, sidestepping whatever the school's resolver had been told to refuse.

That single fact is what turns DNS from a piece of networking plumbing into one of the most useful places you can put a filter, and it is what eventually led me to set my kids' laptops up on NextDNS. Not because I understood the protocol behind it at the time, but because filtering at the DNS level meant one setting on each device did the job, rather than a separate extension for every browser and every kind of content. Understanding DNS properly for Network+ has been filling in exactly why that works — and it turns out DNS is not a single lookup at all. It is a whole hierarchy of servers, a set of record types each doing a specific job, and, more recently, a handful of security extensions bolted on to fix problems the original design never anticipated.

## What DNS Is Actually Doing

Every device you own thinks in IP addresses. Every person typing a website into a browser thinks in names. DNS is the system that translates between the two — you type `fionnlinux.com`, and somewhere behind the scenes a DNS server hands your device the IP address that name actually points to.

That translation is called **resolution**, and it does not happen in one step. Your device does not have a giant list of every domain name in existence. Instead, it asks a **resolver** — often run by your ISP, or in my case NextDNS — to go and find the answer on its behalf. If the resolver already has the answer cached from a recent lookup, it replies straight away. If not, it goes looking.

## The Hierarchy Behind Every Lookup

Finding an unknown domain works by asking increasingly specific servers, starting at the top of a hierarchy:

- **Root servers** know nothing about `fionnlinux.com` specifically, but they know which servers handle `.com`.
- **TLD servers** (top-level domain servers, handling `.com`, `.org`, `.uk`, and so on) know which server is authoritative for `fionnlinux.com`.
- **Authoritative servers** hold the actual records for `fionnlinux.com` and return the real answer.

A resolver doing this whole walk down the hierarchy on your behalf is performing a **recursive** lookup — it does the legwork and hands you back a final answer. The servers it queries along the way are answering **non-recursively**: each one either has the answer or points to the next server down the chain, without doing any further searching itself.

This distinction matters because it is the difference between a server that can tell you where to look and a server that is willing to go and look for you.

## Authoritative vs Non-Authoritative

Once an answer comes back, it is labelled one of two ways. An **authoritative** answer comes directly from the server responsible for that domain — the source of truth. A **non-authoritative** answer comes from a resolver that already had it cached from a previous lookup. It is almost certainly still correct, but it did not come from the horse's mouth this time.

This is also where **primary** and **secondary** servers come in. A primary (or master) server holds the original, editable copy of a domain's records. Secondary servers hold read-only copies, synced from the primary, there to share the load and provide redundancy if the primary goes down.

## Record Types — What Each One Actually Answers

A domain does not have one DNS entry. It has a whole set of them, each type answering a different question:

- **A** — the core record. Maps a name to an IPv4 address.
- **AAAA** — the same job, but for an IPv6 address.
- **CNAME** — an alias. Points one name to another name rather than directly to an address, useful when several subdomains should all resolve wherever the main domain currently points.
- **MX** — mail exchange. Tells the internet which server handles email for a domain.
- **TXT** — a free-text record, commonly used to prove domain ownership or to publish email security policies.
- **NS** — nameserver. States which servers are authoritative for a domain.
- **PTR** — pointer. Does the lookup in reverse, taking an IP address and returning the name associated with it.

## Forward and Reverse Zones

A **forward zone** is the direction covered above — name in, address out. A **reverse zone** runs it the other way, address in, name out, using a PTR record to do it. It comes up in mail server verification, and in troubleshooting where you have an IP address in a log file and want to know what it belongs to.

## The Security Layer Bolted On Top

DNS as originally designed has no concept of trust. A response either arrives or it does not, and nothing verifies it actually came from where it claims to have come from. That gap has been patched, not rebuilt, with three separate additions:

**DNSSEC** adds cryptographic signatures to DNS records, so a resolver can verify that a response genuinely came from the authoritative server and was not tampered with in transit. It solves *authenticity* — is this answer real.

**DNS over HTTPS (DoH)** and **DNS over TLS (DoT)** solve a different problem: *privacy*. A standard DNS query travels in plain text, meaning anyone watching the network traffic — including, historically, your ISP — can see every domain you look up. DoH wraps the query inside an HTTPS connection; DoT wraps it inside its own dedicated encrypted connection. Different transport, same goal: nobody between you and the resolver can read what you are asking.

## The One File Nobody Talks About

Before any of this hierarchy gets involved, most operating systems check one local file first: the **hosts file**. It is a simple, manually editable list of name-to-address mappings that overrides DNS entirely for anything listed in it. It predates DNS as a system, and it survives today mostly as a manual override — useful for testing a site before its DNS record goes live, or for blocking a domain outright at the device level, bypassing any resolver.

On a RHEL-based system it lives at `/etc/hosts`, and it is worth actually opening once just to see it — it is a genuinely simple, plain-text list, nothing hidden or clever about it, editable with root permissions. Seeing it laid out plainly makes the whole concept far less abstract than reading about it in an objectives list.

## A Scenario Worth Walking Through

All of this stays theoretical until you actually have to use it, so here is the kind of situation it applies to directly.

Say a website will not load on one laptop, but loads fine on your phone on the same network. Where do you actually start?

The first useful question is whether this is a DNS problem at all, or something else entirely. A quick check — trying to reach the site by its IP address directly instead of its name — answers that immediately. If the site loads by IP but not by name, the problem is isolated to name resolution, and everything below is the process for narrowing it down further.

From there, the hosts file is worth checking first, precisely because it overrides everything else. If someone has added an entry there — deliberately, or as a leftover from testing something months ago — no DNS record, however correct, will override it. It is the single fastest thing to rule out and the easiest thing to forget exists.

If the hosts file is clean, the next question is whether the laptop is even asking the same resolver as the phone. Different DNS settings on one device — a manually configured resolver, a VPN client silently overriding DNS, a leftover setting from years ago — mean the two devices are not actually asking the same question. This is not hypothetical in my own house: I run Proton VPN on my own devices, which routes my DNS through Proton's own resolvers while it is active, while my kids' devices are configured to use NextDNS directly. Two people on the same home network, at the same moment, can genuinely be asking two different resolvers entirely — and if one of those resolvers has a site blocked, or a stale cached answer, or a record the other resolver does not, that alone would fully explain "it works for me but not for them." Checking which DNS server each device is actually configured to use answers that question directly, rather than leaving it as a guess.

If both devices are pointed at the same resolver and one still fails, the fault likely sits with the record itself, or how it is being served. That is where record type and zone type actually matter in practice, not just as vocabulary: is the domain missing an A or AAAA record for the address type this device happens to be requesting, is the response coming back authoritative or is a stale non-authoritative answer being served from a cache that has not caught up with a recent change, and is DNSSEC validation failing on this device in a way that silently blocks a response the other device accepts without checking.

What I like about laying it out this way is that once you actually know what each piece does, you can work through it methodically rather than guessing. Hosts file, then resolver, then the record itself — check each one off in order, and you land on the actual cause instead of fiddling around at random.

## Why This Matters Beyond the Exam

Understanding DNS properly has changed how I approach troubleshooting, or at least given me somewhere to start rather than just restarting the router and hoping. A device that cannot reach a site might have a perfectly working network connection and a broken DNS configuration underneath it — two different problems that look identical from the outside, both showing up as "the internet isn't working." Knowing the hierarchy, the record types, and where privacy and authenticity get added on top gives me an actual place to look.

If any of this has made you curious enough to actually see DNS filtering in action rather than just read about it, NextDNS is a reasonable place to start. It is free to set up and use for normal home traffic, there is nothing to buy, and you can watch queries and blocks happen in real time from its dashboard, which does more for understanding this than any amount of reading. Worth being aware, if privacy is a particular concern for you, that it is a cloud service — your queries are going to NextDNS's servers rather than staying entirely on your own network, which is a different trade-off to something self-hosted like Pi-hole.
