---
title: "When the Network Itself Is Broken — Troubleshooting Switching and Addressing"
date: 2026-07-23T00:00:00+01:00
draft: false
description: "STP, VLAN misassignment, ACLs, routing, and IP addressing faults, worked through with two related incidents on the same small office network."
tags: ["networking", "comptia", "troubleshooting"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Picture the same small office network again — a couple of switches connecting everything together, a router handling traffic out to the internet, and several VLANs separating staff, guests, and a handful of servers. Two separate incidents on that network, a few weeks apart, are enough to walk through everything this objective actually covers.

## Incident One — The Network That Suddenly Slowed Down

One afternoon, the whole office network becomes sluggish at once, not just one device or one switch — everything.

### STP and Network Loops

With two switches in the building, someone has recently added a second cable between them for redundancy, without realising both switches were already connected once already. That creates a loop — a path where traffic can circulate endlessly between the two switches rather than reaching its actual destination, and a loop like this is exactly what floods a network and slows everything down at once.

**Spanning Tree Protocol (STP)** exists specifically to prevent this. It works by having switches elect a **root bridge** — a single reference point the whole topology is built around — and then deciding the **port roles** and **port states** of every other link based on that election. A redundant link like the one just added gets placed into a blocking state rather than being allowed to actively forward traffic, so the physical loop exists but no traffic loop actually happens.

The fault here turns out to be that STP was never properly enabled on one of the switches, so it had no mechanism in place to block the redundant link. Once STP is correctly running on both switches, the root bridge gets elected, the redundant link is placed into its correct blocking role, and the network settles back down immediately.

### Incorrect VLAN Assignment and ACLs

While investigating the slowdown, a second, smaller issue turns up: a handful of guest devices have full access to the staff VLAN, which should never be possible. A **VLAN** — virtual local area network — is a way of splitting one physical network into separate logical networks, so that devices on different VLANs cannot normally reach each other even though they share the same switches and cabling. Tracing this fault back, a switch port was assigned to the wrong VLAN during a recent change, putting a guest device on the staff network instead of the isolated guest one — an **incorrect VLAN assignment**, plain and simple.

Even once the port is corrected, it is worth checking the **ACLs** meant to enforce that separation in the first place. An **ACL** — access control list — is a set of rules that explicitly allow or block traffic based on things like address or port, and one is typically used to make sure a guest VLAN cannot reach anything on the staff VLAN even if something does slip through. In this case, the ACL controlling what the guest VLAN can reach was written correctly, but the port assignment mistake bypassed it entirely, since the device was never actually being treated as part of the guest VLAN the rule applied to. It is a reminder that a correctly written rule does nothing if the traffic it is meant to control never actually passes through it.

## Incident Two — The New Starters Who Can't Get Online

A few weeks later, several new starters on the same network cannot get online at all, while everyone else works normally.

### Address Pool Exhaustion

The first thing worth checking is whether DHCP even has anything left to hand out. In this case, the **address pool** for that VLAN's scope has genuinely run out — every address in the defined range is already leased to existing devices, with nothing left over for a new one. New devices simply cannot get an address at all, since there is nothing left in the pool to give them.

### Incorrect Default Gateway

One of the new starters does get an address, through a manual override rather than DHCP, but still cannot reach anything outside the local network. Checking the configuration shows the **default gateway** was entered incorrectly — pointing at an address that does not correspond to the actual router. Local traffic on the same subnet works fine, since that never needs the gateway at all, but anything destined elsewhere has nowhere to actually go.

### Incorrect IP Address and Duplicate IP Address

Another new starter's device was set up with a manually assigned address, and it turns out to be outside the subnet the rest of the network actually uses — an **incorrect IP address**, unable to communicate properly with anything on the correct range. A different device, also manually configured, was given an address that happens to already be in use by another machine on the network — a **duplicate IP address**, which typically causes both devices to experience unpredictable connectivity as the network tries to resolve which one actually owns that address.

### Incorrect Subnet Mask

The last device has a correctly assigned address and gateway, but a subnet mask that does not match the rest of the network. An **incorrect subnet mask** changes what a device believes the boundary of its own local network actually is, meaning it may treat some genuinely local addresses as if they were on a different network entirely, or the other way round, breaking communication with devices it should be able to reach directly.

### Route Selection

With individual devices sorted, one further symptom remains: traffic meant for a specific remote office is not arriving. A router's job is to look at where a piece of traffic is trying to go, and decide which direction to send it next — and it does that by checking a **routing table**, which is really just a list of known destinations and which direction leads to each one, similar to a signpost at a junction. Checking that list shows no entry at all for this particular remote office.

Normally, when a destination is not listed specifically, a router falls back on a **default route** — a kind of catch-all instruction meaning "if you don't recognise the destination, send it this way anyway," usually pointing out toward the wider internet. In this case, no default route is configured either. So the router has no specific signpost for this destination, and no fallback instruction for what to do when a signpost is missing — meaning the traffic genuinely has nowhere to go, and stops right there.

## What Both Incidents Share

Every fault across both incidents traces back to the same basic idea: something that was supposed to be defined explicitly — a blocked port, a VLAN assignment, an address, a route — either was not defined at all, or was defined incorrectly. None of these are unusual or complicated problems. They are ordinary configuration details that, left unchecked, produce symptoms ranging from a slow network to a device that cannot get online at all.
