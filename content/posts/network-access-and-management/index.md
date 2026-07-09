---
title: "Getting In and Managing It — VPNs, SSH, and the Other Ways Networks Let You Work Remotely"
date: 2026-07-05T00:00:00+01:00
draft: false
description: "Site-to-site vs client-to-site VPNs, SSH and the other connection methods, jump boxes, and in-band vs out-of-band management — worked through ahead of an actual Ansible project that will need SSH into my own home server and the kids' laptops."
tags: ["networking", "comptia", "security", "homelab"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

I have not actually used SSH yet. I have watched videos on it and understood the concept behind public and private key pairs — the general idea that one key stays secret and one can be shared, and that together they let a connection be verified and encrypted without ever sending a password across the network in the clear. What I have not done is actually sit down and go through the handshake itself, or open a terminal and type `ssh` into it for real. That will change in the next few months when time allows. The next homelab project on my list is using Ansible to manage updates across the kids' laptops from my own machine, the same pattern I have already read through in an Ansible playbook guide — and every single step of that project depends on SSH being the connection method underneath it. Before I actually do it, it is worth understanding properly what SSH is being chosen over, and why.

## VPNs — Connecting Networks, Not Just Devices

A VPN is not one single thing with one single job, and the exam splits it two ways for a reason — the two types solve genuinely different problems.

A **site-to-site VPN** connects two networks to each other, permanently, as if they were one continuous network — the classic example being two office locations that need to share resources securely over the public internet without either side needing to think about it device by device. No individual user connects or disconnects; the tunnel between the two sites is just always there.

A **client-to-site VPN** does the opposite job. Rather than joining two whole networks together, it connects one single device into a network it is not physically on. This is the one I actually use — Proton VPN, though aimed at general privacy rather than reaching a specific home or work network. Where site-to-site is permanent and network-to-network, client-to-site is temporary and device-to-network, switched on and off by the person using it. It also comes in variants worth knowing apart:

**Clientless** access is a VPN that works without any VPN software installed on your device — instead, you connect to it through an ordinary web browser, over a normal HTTPS connection. The browser's own encryption is doing the same job a VPN client's software would normally do, so there is nothing extra to install. The trade-off is what you actually get access to: rather than joining the whole remote network the way installed VPN client software would, a clientless connection usually only gives you specific web-based resources through that portal — a particular internal site or application, not the network as a whole. Logging into a work portal from a browser on a shared library computer, rather than installing anything on it, is a typical example.

**Split tunnel vs full tunnel** is its own contrast, and decides how much of a device's traffic actually goes through the VPN. Split tunnel sends only the traffic meant for the destination network through the tunnel, while everything else — general browsing, other apps — goes out directly over the normal internet connection. Full tunnel routes everything through the VPN, no exceptions, which costs more bandwidth and speed but hides more.

## Connection Methods — The Actual Mechanism

Once you are actually reaching a device or network, there are a handful of distinct ways to do it, and each has a genuinely different shape.

**SSH** is the encrypted command-line session into a remote machine, and the one most relevant to what I am about to do — it is exactly how Ansible reaches every machine in an inventory, including the kids' laptops in the project ahead. Everything typed and returned travels encrypted, which is the whole point of reaching for it over something unencrypted like Telnet.

**GUI** access means a graphical remote session rather than a command line — seeing and controlling the actual desktop of a remote machine, the way Remote Desktop Protocol works. I have actually done this once, using RustDesk, an open source tool, to control an old Windows laptop from my Linux machine. Better suited to anything that genuinely needs a visual interface rather than commands — I needed to see the Windows desktop itself, not just run commands on it.

**API** access is different again — not a person typing commands or looking at a screen at all, but software talking directly to a system through a defined interface, the way an automation script or another application might configure or query a device without a human in the loop at any point.

**Console** access is the one that assumes everything else has failed. It is a direct connection to a device — physically, or through a dedicated console port — that bypasses the normal network path entirely. If a device's network configuration itself is broken, none of SSH, GUI, or API will reach it, because all three depend on the network already working. Console access exists specifically for the situation where the network is the actual problem.

Set side by side, the four fall into two different camps: SSH, GUI, and API all assume the network is working and differ mainly in who or what is on the other end — a person typing commands, a person looking at a screen, or software with no human involved at all. Console stands apart from all three, because it does not make that assumption in the first place.

## Jump Box — One Door, Not Many

A **jump box** (sometimes jump host) is a single hardened server that sits between you and anything sensitive. Rather than every admin having direct access to every internal system, everyone connects to the jump box first, and only from there do they reach the actual servers behind it.

Worth separating from a firewall, since the two can sound similar. A firewall filters traffic by rule, silently allowing or blocking based on things like port or address, and nobody ever logs into it to do their actual work. A jump box is the opposite in that sense — it is somewhere you actually log into and work from, a stepping stone rather than a filter.

The value is concentration. Instead of securing, patching, and monitoring access on every single internal system separately, there is one carefully controlled doorway everything passes through, which is far easier to lock down properly and watch closely than a dozen separate access points scattered across a network.

## In-Band vs Out-of-Band Management

This is the distinction that ties everything above together, and it comes down to one question: does your management connection depend on the same network you are trying to manage?

**In-band management** uses the normal production network to reach and configure a device — the same path regular traffic takes. It is simple and requires no extra hardware, but it has an obvious weakness: if the network itself goes down, or the specific device you need to reach has lost its network configuration, in-band management is unreachable for exactly the same reason everything else is broken.

**Out-of-band management** uses a completely separate path — a dedicated management network, or a device's own console port — that does not depend on the production network being healthy at all. This is why console access matters so much as a connection method: it is frequently the actual mechanism behind out-of-band management, there specifically for the moment when the normal network cannot get you where you need to go.

## Picking the Right Door

What this comes down to, in the end, is that none of these methods are really interchangeable — each one quietly assumes something different has to already be true before it can work. SSH, GUI, and API all need the network itself to be functioning to reach a device at all. Console access does not make that assumption, which is exactly why it exists as its own separate option rather than just another way of doing the same thing. The question worth asking is not which one you prefer, but which one still works once something has already gone wrong.
