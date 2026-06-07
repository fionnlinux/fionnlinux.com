---
title: "The Shape of a Network — and Why It Matters for Security"
date: 2026-06-06
draft: false
description: "Network topology explained from the ground up — the cable runs I did as an electrician, the home network already running off my router, and why a network's structure decides how far a threat can spread."
tags: ["networking", "comptia", "security", "homelab", "linux"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Before I understood what a network topology was, I had already built one.

As an apprentice electrician, one of the jobs that came up regularly was cabling new office builds. I would run Cat5 from the main comms cabinet through trunking along the walls and ceiling, down to network outlets at each desk — one drop per workstation, clean and neat, so everyone had a reliable wired connection. I did not think much about it at the time. It was a trade job. Pull the cable, terminate the ends, move on.

Studying for Network+ years later, I reached the section on three-tier network architecture and read about the access layer — the part of the network where end devices connect. And I recognised it immediately. That is what I had been building. I was installing the access layer of a structured enterprise network without knowing the name for it.

That is what network topology actually is: the structure of how devices connect to each other, and why that structure matters. Not a diagram you memorise for an exam, but a set of decisions that determines how a network scales, how it performs, and crucially how far a threat can spread if something goes wrong.

## The Home Network Realisation

When I first started looking into running a homelab, I was confused about what I actually needed. Did I need racks? Switches? Piles of cables? How would all the devices communicate if most of them were wireless?

Then it became obvious.

Every device in a home network already connects to one central point: the router. Whether it is wired via Ethernet or connected over Wi-Fi, every device on the network reaches everything else through that single device. The router is acting as a Layer 3 device — making routing decisions about where traffic should go — and also as a Layer 2 switch, handling communication between devices on the local network. The physical medium does not matter. You are already on a network, and it already has a topology.

That topology is called a **star**.

## Star Topology

In a star topology, every device connects to a single central point. At home that is your router. In a small office it might be a dedicated switch, with the router sitting upstream of it.

The star is the dominant topology for local area networks because it is simple to manage and fault-tolerant at the edge. If one device's connection fails, only that device is affected. The rest of the network keeps running.

The trade-off is that the central device is a single point of failure. If the switch or router goes down, the whole network goes with it. That is manageable at home. In an enterprise environment it drives the design decisions that produce more resilient architectures.

When I look at the cable trays running along the ceiling of the manufacturing building where I work now — the switches mounted at intervals, the access points hanging below them, the cables dropping to workstations and machinery on the floor — I see a star topology, replicated across the building, with each switch acting as the central point for the devices connected to it.

## Point-to-Point and Hub and Spoke

A **point-to-point** topology is the simplest network there is: a single direct link between two devices, and nothing else. Picture a private phone line running between two buildings — only those two ends are connected, and the line is theirs alone. Nothing else shares it, so it is reliable and completely predictable.

The catch is that it only ever connects two points. If you wanted to link ten offices this way, you would need a separate dedicated line between every single pair of them, which quickly becomes impractical and expensive. It is perfect for joining two fixed locations and poorly suited to anything larger.

**Hub and spoke** takes a central hub with connections radiating outward to spoke locations. It looks like a star, and the two terms are sometimes used interchangeably, but hub and spoke usually describes connectivity across a wider area — branch offices connecting back to a headquarters. All traffic between the spokes passes through the hub. That can make the hub a bottleneck, but it also gives you a single, natural place to apply security rules and monitor what is going where.

## Three-Tier Architecture

The three-tier model is the standard framework for designing larger enterprise networks. It divides the network into three distinct layers, each with a defined job.

The **access layer** is where end devices connect — workstations, phones, printers, wireless access points. This is the layer I was building as an electrician. Every Cat5 run I pulled terminated at the access layer.

The **distribution layer** sits above the access layer and acts as the middle manager. Physically, it is a smaller number of more powerful switches — multilayer switches, meaning switches that can also route traffic, not just forward it — typically sitting in a communications room that serves a whole floor or section of a building. Each one gathers the traffic coming up from many access-layer switches below it.

This is where the decisions get made. The distribution layer determines what is allowed to go where — which devices can talk to which, how traffic is routed between different VLANs, and what the security rules are. The access control lists and routing policies live here. If a device tries to reach somewhere it should not, this is the layer that stops it. Think of it as the checkpoint between the desks and the rest of the building.

The **core layer** is the high-speed backbone of the whole network. Physically, it is a small number of very fast, very powerful switches — usually the most expensive networking hardware in the building, and often deployed in redundant pairs so that if one fails the other keeps everything running without interruption. They tend to live in the main equipment room alongside the servers.

Its only job is to move large amounts of traffic between distribution-layer devices as fast as physically possible. It does not stop to check rules or make security decisions — that work has already been done lower down. Think of it as the motorway: no junctions, no traffic lights, just maximum speed between major points. Keeping the rule-checking out of the core is deliberate, because anything that slows the backbone down slows the entire network.

Together the three tiers give large networks a structured, scalable foundation. Each layer has a clear responsibility, which makes the network easier to troubleshoot, expand, and secure.

## Collapsed Core

In smaller enterprise environments, running a fully separate core and distribution layer is unnecessary overhead — a lot of expensive hardware doing very little. The **collapsed core** design merges the two upper layers into one. Instead of a set of core switches and a separate set of distribution switches, you have a single pair of multilayer switches that do both jobs at once: applying the routing and policy the distribution layer would handle, while also acting as the fast backbone the core would provide.

In practice that means a two-tier network — the access layer at the bottom where devices plug in, and the combined core-and-distribution layer above it. You keep the clear separation of the access layer while collapsing everything above it into a single tier. Most small and mid-sized businesses run some variation of this. It gives you enough structure to stay manageable and secure, without the cost and complexity of a full three-tier deployment that really only the largest networks need.

## Spine and Leaf

The spine and leaf topology is designed for data centres, where the traffic behaves very differently from an ordinary office network.

To understand why this topology exists, it helps to know how data centre traffic differs. In a traditional network, most traffic travels what engineers call **north-south** — up and down between users and the outside world. A person at their desk requests a web page, the request leaves the building, and the response comes back. The three-tier model is built around exactly this pattern.

A modern data centre is different. A huge amount of its traffic travels **east-west** — sideways, between the servers and applications sitting inside the data centre itself, constantly talking to one another. One application asks another for data, which asks a third, all without anything ever leaving the building. The three-tier model handles this badly, because that sideways traffic has to climb all the way up the hierarchy and back down again just to reach a neighbour one rack over.

Spine and leaf fixes this by flattening everything out. There are only two kinds of switch: **leaf** switches, which everything connects to, and **spine** switches, which the leaf switches connect to. The rule is simple — every leaf connects to every spine, and the spines do not connect to each other. The payoff is that any two points in the network are always the same short distance apart: a fixed two hops, up to a spine switch and straight back down to the destination leaf. No connection is ever further away than any other, so performance stays predictable no matter how large the network grows. Need more capacity? Add another spine or leaf switch without redesigning anything.

I will be straight with you: this is the one topology I understand from study rather than experience — I have never worked near data centre infrastructure. But the underlying idea is clear enough, and it leads directly into something I do find intuitive: the security side of east-west traffic.

## North-South and East-West Traffic — Why Topology Is a Security Decision

Most traditional network security is built around the perimeter. A firewall sits at the boundary, inspecting traffic entering and leaving the network. This is **north-south** thinking — traffic that crosses the edge of the network.

For a long time this was enough. Threats came from outside. You locked the front door and felt reasonably secure.

The problem is that once an attacker is inside the network, those perimeter controls do not help. What matters then is how freely traffic can move **east-west** — sideways, from one internal device to another. A compromised workstation that can talk freely to every other device on the network is an attacker's dream. They can move from the initial foothold to more valuable targets without ever touching the perimeter again.

This is lateral movement, and it is one of the most significant threats in modern enterprise environments.

The defence is network segmentation — designing the topology so that different parts of the network cannot communicate freely with each other. VLANs are the most common tool for this. A device on the guest VLAN cannot reach devices on the operations VLAN. A compromised smart device cannot pivot across to the server running critical systems.

Topology determines how far a threat can spread. A flat network — everything connected, everything able to reach everything — hands an attacker the whole estate the moment they get one foothold. A properly segmented network turns that same foothold into a contained incident.

This is exactly why I want to build an OPNsense firewall with VLAN segmentation — first inside a virtual machine to learn it properly, and eventually as my actual home network setup. It is the same principle the enterprise models are built on, applied at a scale I can run and experiment with myself.

## Topology Is Not Abstract

The cable runs I did as an apprentice, the manufacturing network I can see above me at work, the home network quietly running off my router before I ever thought to look at it — all of it maps to the same handful of concepts. Different scales, same shapes.

That is the encouraging part, and it is worth holding onto if you are learning this too. You do not need a data centre or a rack of enterprise switches to understand topology. You are almost certainly sitting inside a working example of one right now — the network in your home is a real star topology you can reason about, poke at, and learn from today.

And when you want to go further, you do not need to buy hardware to do it. The project I am most looking forward to is exactly that: spinning up OPNsense in a virtual machine and building VLANs in software — carving a network into zones and controlling what is allowed to cross between them — all on a laptop, for free. It is the same principle the three-tier and segmentation models are built on, just at a scale that fits on your own machine.

I will be straight about where I am with it. Right now I am in the planning and theory stage, getting the concepts solid before I build. That is the next project once time allows, and I am genuinely looking forward to it — there is something satisfying about learning the shape of a thing first and then watching the theory turn into something real. If you are studying networking too, I would recommend the same approach: understand the structure, then build a small version yourself. If you are anything like me then you learn it properly the moment you stop reading about it and start building it.
