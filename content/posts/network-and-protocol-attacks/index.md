---
title: "The Attacks Aimed at the Network Itself — DoS, Spoofing, and Getting Past VLANs"
date: 2026-07-16T00:00:00+01:00
draft: false
description: "DoS/DDoS, VLAN hopping, MAC flooding, ARP and DNS poisoning/spoofing, rogue devices, evil twin, and on-path attacks — what each one actually does to a network, grounded in a long-term plan to segment my own network with OPNsense."
tags: ["networking", "security", "comptia"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Part of the long-term homelab plan is running OPNsense on a Protectli box, properly segmenting my own network into separate VLANs rather than everything sitting on one flat network. Working through this section of Network+, VLAN hopping was the one that actually caught my attention, because building segmentation without understanding the attack it is meant to defend against felt like the wrong order to learn things in. It sits alongside a handful of other attacks aimed at the network itself rather than at a person, all covered here together rather than one at a time.

## VLAN Hopping

VLANs exist to logically separate traffic that shares the same physical switches, so that devices on one VLAN cannot normally see or reach devices on another. VLAN hopping is specifically about defeating that separation, and it works through two distinct methods.

**Switch spoofing** takes advantage of a protocol called DTP (Dynamic Trunking Protocol), which some switches use to automatically negotiate whether a port should become a trunk port — the kind of port that carries traffic for multiple VLANs at once. An attacker's device pretends to be a switch itself, negotiates a trunk connection with a legitimate switch that has auto-negotiation switched on, and once that trunk exists, gains access to every VLAN carried across it.

**Double tagging** is a genuinely different technique. The attacker sits on what is called the native VLAN — the one a trunk port treats as untagged by default — and sends a frame carrying two VLAN tags stacked on top of each other. The first switch in the path strips off the outer tag, since it matches the native VLAN it already expects, and forwards the frame onward still carrying the second, inner tag. That inner tag is the actual target VLAN, and the frame ends up delivered somewhere it should never have been allowed to reach. It only works one direction — the attacker can send a frame in, but nothing can be sent back the same way — but that is still enough to inject traffic into a VLAN that was supposed to be isolated.

The impact in both cases is the same: segmentation that was assumed to be a hard boundary turns out not to be one, and traffic crosses between VLANs that should never have been able to communicate at all. Knowing this exists is exactly why disabling automatic trunk negotiation on ports that do not need it is a real, practical step, not just theory — something to actually configure properly when the OPNsense segmentation project happens, rather than assume VLANs alone are enough.

## Denial-of-Service

**DoS** (Denial-of-Service) is an attack aimed at making a system or service unavailable, rather than trying to steal or alter anything. It works by overwhelming a target with more requests or traffic than it can actually handle, until legitimate users simply cannot get through. **DDoS** (Distributed Denial-of-Service) is the same goal achieved from many sources at once, usually a large number of compromised devices all directed at the same target simultaneously, which makes it both far more powerful and far harder to block, since there is no single source to simply cut off.

The impact is specifically to availability, one third of the CIA triad covered in an earlier post — nothing is stolen or changed, but the service stops being reachable for exactly as long as the attack continues.

## MAC Flooding

Switches keep a table mapping MAC addresses to the physical ports they are connected through, which is how a switch knows to send traffic only out the correct port rather than everywhere at once. **MAC flooding** deliberately overwhelms that table with an enormous number of fake MAC addresses, until it has no room left to track legitimate ones.

The impact is that many switches, once their table is full, fail open rather than closed — they start behaving like a hub, broadcasting traffic out every port rather than just the correct one. That turns a network that was supposed to keep traffic contained to specific ports into one where an attacker can suddenly see traffic meant for other devices entirely.

## ARP Poisoning and ARP Spoofing

ARP (Address Resolution Protocol) is what maps an IP address to a MAC address on a local network, and it was never built with any way to verify that a reply is genuinely coming from who it claims to be from. **ARP spoofing** is sending a forged ARP reply, claiming a particular IP address belongs to the attacker's own MAC address rather than the real device's. **ARP poisoning** is the result of doing this successfully — other devices on the network update their own ARP tables with the false mapping, genuinely believing it.

The impact is that traffic meant for the real device gets sent to the attacker instead, without anything on the network flagging it as wrong, because ARP has no built-in way to catch a lie like this.

## DNS Poisoning and DNS Spoofing

This pairs with ARP in the same way — **DNS spoofing** is sending a forged DNS response, and **DNS poisoning** is the resulting corrupted entry sitting in a resolver's cache, now confidently handing out the wrong address to anyone who asks. Covered properly in an earlier post on how DNS actually works, this is exactly the kind of forged response DNSSEC exists to catch, by verifying a response genuinely came from the authoritative server rather than an attacker sitting somewhere in between.

The impact is a device being sent somewhere it never intended to go — potentially a convincing fake version of a real site — while believing the whole time that DNS resolved the name correctly.

## Rogue Devices and Services

A **rogue DHCP** server is an unauthorised device on the network handing out its own DHCP leases, potentially pointing devices at the wrong gateway or DNS server entirely, silently redirecting their traffic without them ever knowing DHCP was the point where it went wrong. A **rogue AP** is an unauthorised wireless access point, plugged into the network without permission, giving anyone who connects to it a route onto a network they were never supposed to be able to reach.

The impact of either is the same shape: something nobody approved is now sitting inside a boundary that was supposed to keep unapproved things out.

## Evil Twin

An **evil twin** is a fake wireless access point specifically set up to look identical to a real, trusted one — same network name, sometimes even a cloned MAC address — so that a device connects to it automatically or a person connects to it by mistake, believing it to be the legitimate network. Once connected, every bit of traffic passes through the attacker's access point first.

The impact is a direct loss of confidentiality, since the attacker is now positioned to see everything that passes through, on a network the victim genuinely believes is the real one.

## On-Path Attack

An **on-path attack** (formerly commonly called man-in-the-middle) is the broader category several of the attacks above actually belong to — ARP spoofing, DNS spoofing, and an evil twin are all specific ways of achieving the same underlying position: getting in between two parties who believe they are communicating directly with each other, when in fact everything is passing through the attacker first.

The impact depends entirely on what the attacker chooses to do once in that position — simply observe traffic passing through, or actively alter it before passing it on — but the defining feature of an on-path attack is the position itself, not any one specific technique used to get there.

## The Pattern Running Through All of This

Nearly every attack above works by exploiting a protocol that was never designed with any way to verify trust — ARP, DTP, DHCP, and originally DNS all just accept information at face value, with nothing built in to check whether it is genuine. That is the same thread running through segmentation, monitoring, and the security extensions covered in earlier posts: none of these protocols protect themselves, so something else always has to.
