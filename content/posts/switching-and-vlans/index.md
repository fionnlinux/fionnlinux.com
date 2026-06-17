---
title: "Switching and VLANs — Dividing a Network on Purpose"
date: 2026-06-17T00:00:00+01:00
draft: false
description: "VLANs, trunking, spanning tree, MTU, and port mirroring — the switch features worth understanding and when you'd reach for each."
tags: ["networking", "comptia", "security", "vlan", "switching", "homelab"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Knowing what a switch feature is called and knowing when to use it are two different things. This post covers the main switching features worth understanding — VLANs, trunking, spanning tree, port mirroring, and a few others — and tries to frame each one as a decision: what problem does it solve, and when would you reach for it?

It's also the topic I'm most looking forward to putting into practice. One of the things I want at home is proper network segmentation: separate VLANs for the children's devices, the homelab, and anything I don't fully trust, with firewall rules in OPNsense controlling what's allowed to talk to what. The switch is where that segmentation starts.

---

## VLANs — one switch, many networks

**The scenario:** you want to put two groups of devices on the same switch but stop them from talking to each other, without buying a second switch.

A VLAN (Virtual LAN) splits a single physical switch into separate logical networks. Ports in VLAN 10 are isolated from ports in VLAN 20 — they can't reach each other directly, and a broadcast from one doesn't reach the other. For me, that's children's devices in one VLAN, homelab in another, and OPNsense with firewall rules deciding what's allowed between them. The segmentation is enforced at the switch.

**Switch Virtual Interface (SVI)** — a VLAN is a layer 2 thing: it separates traffic on a switch, but the VLAN itself has no IP address. An SVI is how you give a VLAN an IP presence on the switch. If you want to reach the switch over the network to manage it, you create an SVI for the management VLAN and assign it an IP address — that's what you connect to. On a layer 3 switch (one that can route traffic as well as switch it), SVIs also let VLANs communicate with each other: each VLAN gets an SVI that acts as its default gateway, and the switch handles the routing between them internally.

---

## Interface configuration — how ports connect to VLANs

Each port on a switch is configured depending on what's connected and what it needs to carry.

**Access port** — a port that belongs to one VLAN and connects to a single end device: a laptop, a printer, an IP camera. The device just sees normal Ethernet; it has no idea VLANs are involved. If you're putting a device on VLAN 20, you configure that port as an access port assigned to VLAN 20 and it's done.

**Trunk port** — a port that carries multiple VLANs over one cable. You'd use this between two switches, or between a switch and a router or firewall, where several VLANs need to cross a single link. The trunk uses **802.1Q tagging** to keep them separate — a small tag is added to each frame identifying which VLAN it belongs to, so the device at the other end knows how to handle it. That's how one cable between my switch and OPNsense can carry traffic from several different VLANs at once.

**Native VLAN** — 802.1Q trunks tag all traffic to identify which VLAN it belongs to, with one exception: the native VLAN travels untagged. The reason it exists is backward compatibility — older devices that don't understand 802.1Q tagging can still function on an 802.1Q trunk as long as they're on the native VLAN, because their untagged traffic just passes through. By default that's VLAN 1. The security concern is that an attacker can craft frames that appear to belong to a different VLAN — a technique called VLAN hopping. Standard practice is to change the native VLAN away from VLAN 1 and keep real traffic off it.

**Voice VLAN** — when a VoIP phone and a workstation share a single switch port, a voice VLAN separates the phone's traffic onto its own VLAN automatically. Voice traffic is sensitive to delays, so keeping it isolated lets you prioritise it — call quality doesn't suffer when the rest of the network is busy.

**Link aggregation (LACP)** — the scenario is needing more bandwidth between two switches, or wanting redundancy without relying entirely on spanning tree. LACP (Link Aggregation Control Protocol) bundles multiple physical links into one logical link. If one cable fails the others keep working. You'd see this between core switches, or between a server and a switch where one link isn't enough.

**Speed and duplex** — speed is the interface rate (100 Mbps, 1 Gbps, 10 Gbps). Duplex is whether a link can send and receive at the same time. Full duplex does both simultaneously; half duplex does one direction at a time, like a walkie-talkie. Most modern links negotiate these settings automatically, but if one end is configured manually and the other isn't, they can end up disagreeing. From what I've read, a mismatch is one of the first things worth checking when a link appears to be up but is performing poorly.

---

## Spanning tree — preventing broadcast storms

**The scenario:** you've added a second cable between two switches for redundancy, and the network has just fallen over.

Without spanning tree, a loop in the network is fatal. A broadcast frame goes out in all directions, hits the loop, multiplies, and keeps circling indefinitely. The switches become overwhelmed almost immediately. This is a broadcast storm, and it can bring down an entire network very quickly.

Spanning Tree Protocol (STP) prevents this by finding all the possible paths in the network and deliberately blocking the ones that create loops, keeping them in reserve for when the active path fails.

How it decides what to block:

- All the switches elect a **root bridge** — the reference point the whole topology is built around. The switch with the lowest bridge priority wins; if priorities tie, the lowest MAC address breaks it.
- Every other switch finds its shortest path back to the root bridge and keeps that path active. Any path that would form a loop is blocked.
- If the active path fails, a blocked path is brought back up.

A port works through several states before it starts forwarding traffic: **blocking**, **listening**, **learning**, then **forwarding** (ports can also be **disabled**). Working through those states takes time — classic STP can take anywhere from about 30 up to 50 seconds to settle after a change. **Rapid Spanning Tree (RSTP)** is the updated version that converges much faster and is what you're more likely to encounter now.

If STP isn't running or is misconfigured, the result can be a broadcast storm — a network going down the moment a redundant link is added, with no obvious cause until you know what to look for.

---

## MTU and jumbo frames

MTU (Maximum Transmission Unit) is the largest packet a link will carry. Standard Ethernet is 1500 bytes. **Jumbo frames** increase that to around 9000 bytes, which reduces overhead when large amounts of data are being moved — fewer, larger frames instead of many small ones. The catch is that every device along the path needs to be configured to support the larger size. If one end is set and the other isn't, you get dropped or fragmented traffic rather than any speed improvement.

---

## Port mirroring — watching traffic without touching it

**The scenario:** you want to capture and analyse the traffic on a switch port, without placing a device physically in the path of that traffic.

Port mirroring copies traffic from a port or a whole VLAN to a separate monitoring port where an analyser can watch it. That monitoring port can run Wireshark, an IDS, or something like Security Onion — which I've been looking at as an option for network monitoring on the homelab. Security Onion can do full packet capture and intrusion detection, but it needs to actually see the traffic to do anything with it. Port mirroring is how you feed it. Without mirroring configured, a passive monitoring device sees nothing.

---

## Picking the right feature

For the exam, "given a scenario" means being handed a situation and identifying which feature addresses it. Running through the main ones:

- Isolating devices from each other on one physical switch → VLAN
- Carrying multiple VLANs over one cable between switches → trunk port with 802.1Q
- Separating and prioritising voice traffic → voice VLAN
- Needing extra bandwidth or redundancy between two switches → link aggregation
- A link is up but performing poorly → check speed and duplex settings
- Adding a redundant switch link caused the network to fail → spanning tree wasn't running or was misconfigured
- A monitoring tool needs to see network traffic passively → port mirroring

Most of this post is also the groundwork for my own plans — the segmentation I want at home starts here, at the switch.
