---
title: "Locking a Network Down Properly — Hardening, Access Control, and Zones"
date: 2026-07-19T00:00:00+01:00
draft: false
description: "Device hardening, NAC, key management, ACLs, and network zones — applied to a small office network, showing where each control actually fits rather than just what it is."
tags: ["networking", "security", "comptia"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Security controls make far more sense applied to something concrete than listed in the abstract, so this post works through a single example network rather than defining each control on its own.

Picture a small office. There is a router connecting it to the internet, a switch that staff plug their laptops into, a wireless network, a public-facing web server that customers reach from outside, and a reception area where a network port sits out in the open on the wall. That one small setup is enough to show where every part of this objective actually applies.

## Hardening the Devices

The first exposure in that office has nothing to do with clever attacks — it is the equipment arriving insecure by default. The router ships with a factory username and password, and those defaults are public knowledge for any given make and model. Left unchanged, that is effectively no password at all, since anyone who identifies the device already knows how to log into it. **Changing default passwords** closes that gap immediately.

The same router and switch also run services and expose ports that this office does not use. Every one of those left active is another way in that nobody is monitoring. **Disabling unused ports and services** shrinks the device down to only what it actually needs to do, and everything switched off is one less thing an attacker can reach for. Together, these two steps are **device hardening** — reducing how much of each device is exposed before anything else is even considered.

## Deciding What Is Allowed to Connect — NAC

Now the port on the reception wall becomes the problem. Anyone waiting in that area could unplug the display sitting there and plug in their own laptop, landing straight on the internal network. Network access control exists for exactly this.

**Port security** solves the reception port directly — the switch port is configured to accept traffic only from one specific, known MAC address, so an unfamiliar laptop plugged into it simply gets nothing.

**MAC filtering** applies the same principle more broadly, allowing or denying devices across the network against a defined list of permitted MAC addresses rather than locking down one single port. It raises the bar, though it is worth knowing a MAC address can be spoofed, so it is not an absolute barrier on its own.

**802.1X** is the stronger option, requiring any device — on the reception port, on staff switch ports, or on the wireless — to actually authenticate and prove its identity before being granted access at all. For an office that wants more than a static list of allowed hardware, this is the proper answer.

## Separating the Web Server — Zones

The public web server creates a different problem. It has to be reachable from the untrusted internet by design, which means it is far more exposed than anything else in the office. Putting it on the same internal network as the staff laptops would mean that if it is ever breached, the attacker is already sitting right next to everything else.

The fix is dividing the network into **zones** by how much each part is trusted. The staff laptops sit in the **trusted** internal zone; the internet outside is **untrusted** by default. The web server goes into a **screened subnet** — often still called a DMZ — deliberately placed between the two. It stays reachable from outside, but it is walled off from the internal network, so a compromise of that exposed server does not hand the attacker the staff machines along with it.

## Enforcing the Boundaries — Security Rules

Zones only mean something if something actually enforces what may cross between them, and that is the role of security rules.

**Access control lists (ACLs)** are the rules that explicitly permit or block traffic based on criteria like port or address — for instance, allowing outside traffic to reach the web server on the ports it serves, while blocking any attempt from that same server to reach back into the trusted internal network.

**URL filtering** controls which web addresses users inside the office can reach, blocking known-bad or unwanted sites by their address.

**Content filtering** goes further still, inspecting the actual nature of the traffic — the kind of file or content being transferred — rather than only where it is headed.

## Protecting the Keys — Key Management

The web server almost certainly uses encryption and a certificate so that customer connections to it are secure. But that protection is only ever as strong as the handling of the keys behind it. **Key management** covers how those keys are generated, stored, rotated, and eventually retired. A strong encryption setup guarding the connection means nothing if the private key is stored carelessly or never changed, because whoever obtains that key can undo all of it. This runs quietly underneath the visible controls, but it holds them up.

## Why They Only Work Together

Applied to that one small office, the pattern becomes obvious: no single control secures it. Hardening the router does nothing about the open reception port. Locking down that port does nothing for the exposed web server. Zoning the web server off means nothing if no rules enforce the separation, and encrypting its traffic means nothing if the keys are badly kept. Each control closes one specific gap, and a network is only actually secure when enough of them hold at the same time that no single weakness reopens the rest.
