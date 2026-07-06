---
title: "The Service Running Every Time I Connect to a Network — DHCP, SLAAC, and Keeping Clocks in Sync"
date: 2026-07-03T00:00:00+01:00
draft: false
description: "DHCP reservations, scope, lease time, relay, SLAAC, and why NTP, PTP, and NTS all exist — the parts of network addressing that happen automatically, explained by someone who has never once had to configure any of them."
tags: ["networking", "dhcp", "comptia", "homelab"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

DHCP keeps coming up throughout my CompTIA studies, right from A+ through into Network+. It shows up constantly around homelab setups and self-hosted services — anything that needs a fixed, predictable IP address to keep working, alongside a warning that DHCP will happily hand that same address to something else the moment a lease renews and the device's address quietly changes underneath it. I read about that problem several times before I actually understood what was causing it, or that there was a whole set of decisions being made automatically every single time a device joins a network.

Studying it properly for Network+ has been the first time I have actually looked at what that invisible process is doing.

## What DHCP Is Actually Deciding

When a device joins a network, it needs an IP address, and it needs to know a handful of other things too — which router to use as its gateway, which DNS server to ask, how long it is allowed to keep that address before it has to check in again. DHCP (Dynamic Host Configuration Protocol) is the service that hands all of that out automatically, rather than someone typing it into every single device by hand.

The process is a short back-and-forth, commonly remembered as DORA:

- **Discover** — the device broadcasts onto the network asking if anything can give it an address.
- **Offer** — a DHCP server replies with an available address and the accompanying settings.
- **Request** — the device asks to actually use that specific offer.
- **Acknowledge** — the server confirms, and the device is now configured.

All of that happens in the background before you have even opened a browser.

## Scope, Lease Time, and Exclusions

The DHCP server is not handing out addresses randomly — it works from a defined **scope**, a range of addresses it is allowed to give out on a given network. Outside that range is off limits for DHCP entirely.

Every address it hands out also comes with a **lease time** — a countdown, not a permanent assignment. A device holds an address for that duration, then has to renew it before the lease expires, or risk losing it back to the pool for another device to be given instead. This is why an address a device had yesterday is not guaranteed to be the same one it gets today.

Within that scope, certain addresses can be carved out entirely using **exclusions** — addresses the server is told never to hand out, typically because something on the network already uses that address permanently and manually, and a collision would break it.

## Reservations and Options

A **reservation** is the exception to DHCP's normal dynamic behaviour. Instead of letting a device get whatever address happens to be free, a reservation ties a specific device — identified by its MAC address — to the same IP address every single time it connects. The address is still being handed out by DHCP, so it is not a manual static configuration on the device itself, but the outcome is the same address, permanently, for that one device.

This is exactly the problem I kept reading about without understanding it. A homelab service or self-hosted app that needs to always be reachable at the same address is broken by exactly this behaviour — the address it has today is not guaranteed to be the address it still has after the next lease renewal, and everything pointing at the old address quietly stops working. A reservation is the actual fix for that. DHCP still does the handing out, so nothing has to be configured manually on the device itself, but the server is told to always give that one specific device — identified by its MAC address — the same address, every time. I have not set one up myself yet, but I finally understand why every homelab guide I have read mentions them.

**Options** are the extra settings bundled into a DHCP response beyond just the address itself — things like which DNS servers to use, which domain name to append, or which time server to point at. The address is the headline, but the options are doing a lot of the actual configuration work.

## Relay and IP Helper

DHCP's discovery step relies on a broadcast, and broadcasts do not normally cross from one subnet to another — routers block them by design. That is a problem on any network with more than one subnet, because a device on subnet B has no way to broadcast its way to a DHCP server that only exists on subnet A.

A **relay agent**, sometimes called an **IP helper**, is configured on the router sitting between the subnets. It listens for DHCP broadcasts on its own subnet, and forwards them directly to the DHCP server elsewhere on the network, then relays the reply back. It exists purely to solve the fact that DHCP's discovery process assumes everyone is shouting on the same local segment, which is not true on any network with real structure to it.

## SLAAC — DHCP's IPv6 Counterpart

**SLAAC** (Stateless Address Autoconfiguration) does a similar job to DHCP but for IPv6, and in a genuinely different way. Rather than a central server handing out addresses from a defined pool and tracking who has which lease, a device using SLAAC constructs its own address itself, using information advertised by the router on the network plus its own hardware details.

The word "stateless" is doing real work in that name. There is no server keeping a table of who has been given what, the way a DHCP server does. Each device works its address out independently and just starts using it, which is a genuinely different model of address assignment than the request-and-lease pattern DHCP uses throughout.

## Time Protocols — Why the Clock Matters

Once devices actually have addresses, there is a separate but related problem: making sure they all agree on what time it is. That sounds almost trivial, until you consider what breaks when it is wrong — certificates that check their own validity against the current date, logs across multiple servers that need to be compared in order during an incident, scheduled jobs that need to fire when they are supposed to. A network where every device has a slightly different idea of the time is a network where a lot of things quietly stop working correctly.

**NTP** (Network Time Protocol) is the standard answer — a hierarchy of time servers, each syncing from a more authoritative source above it, all the way up to servers connected to atomic clocks or GPS. Most devices on most networks are running NTP without anyone ever having configured it directly, similarly to DHCP.

**PTP** (Precision Time Protocol) exists for situations where NTP's accuracy, which is generally good enough for normal use, is not tight enough — environments needing timing precision down to microseconds or better, such as financial trading systems or industrial control setups where the order of events genuinely matters at that resolution.

**NTS** (Network Time Security) addresses a gap in the original NTP design: authentication. Standard NTP has no way to verify that a time update genuinely came from a trusted source rather than being spoofed by something on the network trying to feed a device the wrong time deliberately. NTS adds that verification, the same category of problem DNSSEC solves for DNS — confirming a response is authentic, not just present.

## Reading a Fault Once You Know What's Behind It

None of this is something I have configured myself, but understanding what each piece is actually responsible for gives a much clearer idea of where to look if something breaks.

If a device cannot get onto the network at all, that points toward DHCP itself. The scope might genuinely be exhausted — every address in the range already leased out to other devices, with nothing left in the pool to hand out to a new one.

If a device does get an address and can technically join the network, but cannot resolve websites properly or reach the right gateway, that points toward DHCP options rather than addressing at all. The device has a valid address; it is the DNS server or gateway bundled into that lease which is wrong, not the address itself.

If every device is online and reachable, but certificates are failing validation, scheduled jobs are firing at the wrong moment, or logs from different servers do not line up when compared, that points away from addressing entirely and toward time sync instead — NTP, PTP, or NTS being wrong, blocked, or misconfigured somewhere.

Three different symptoms, three genuinely different services quietly responsible for each one. Knowing which is which turns a vague "it's not working" into an actual place to start looking.
