---
title: "Where Networking Is Heading — SDN, Zero Trust, SASE, IPv6 and IaC"
date: 2026-06-13
draft: false
description: "The last objective in Section 1 of Network+ — a tour of the modern concepts shaping how networks are built and managed: SDN, VXLAN, Zero Trust, SASE, IaC and IPv6."
tags: ["networking", "comptia", "cloud", "infrastructure-as-code"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

This is the last objective in Section 1 of the Network+, and it is a different kind of topic from the rest. Where earlier objectives covered the fixed pieces — cables, addresses, the OSI model — this one is about direction. It asks you to summarise the concepts shaping how modern networks are built and run, most of which lean heavily toward cloud and automation.

That makes it the right note to end the section on, because it is where networking meets the cloud direction I am heading in. Right now I know a bit more about the Infrastructure as Code side than the rest, because that is the part that has caught my interest while learning Linux and automation — so I have gone deeper on it here. The others are concepts I am still building a proper picture of.

## Software-Defined Networking (SDN)

Traditionally, every switch and router makes its own decisions about where traffic goes. The logic that decides (the control plane) and the part that actually moves the packets (the data plane) both live inside each box. To change how the network behaves, you configure devices one at a time.

SDN separates those two jobs. The decision-making is pulled out into a central controller — software — while the physical devices are left to do nothing but forward traffic according to what the controller tells them. One place to set policy, and the whole network follows. That central control is the core idea worth holding on to.

For the exam, the related terms to recognise:

- **Application aware** — the network can make decisions based on the type of application traffic, prioritising what matters.
- **Zero-touch provisioning** — a new device can be plugged in and configured automatically from the central controller, with no manual setup on the device itself.
- **Transport agnostic** — it does not care what underlying connection carries the traffic (broadband, fibre, cellular); the software layer works across whatever is available.
- **Central policy management** — one place to define rules, applied everywhere, rather than configuring each device by hand.

### SD-WAN

SD-WAN is SDN applied to the wide area network — the long-distance connections that link separate sites, such as branch offices connecting back to a head office. The same central, software-driven control is used to manage those connections, choosing the best path across whatever links are available rather than relying on one expensive dedicated line set up by hand. The four terms above all apply directly to it.

## VXLAN — Virtual Extensible LAN

VXLAN solves a problem rooted in the limits of traditional VLANs, so it is worth understanding the limit first.

A VLAN is identified by a number called a VLAN ID. That ID is stored in a 12-bit field. Twelve bits gives you 2 to the power of 12 — which is 4,096 possible values. In practice that caps a network at around 4,094 usable VLANs. For a home or a single office that is far more than enough. For a cloud provider running thousands of separate customers on shared hardware, 4,094 segments is nowhere near enough.

VXLAN raises that ceiling dramatically. It uses a 24-bit identifier instead of 12-bit, which gives roughly 16 million possible segments. That scale is the whole point — it is why cloud platforms rely on it underneath.

The two exam terms:

- **Layer 2 encapsulation** — VXLAN takes a Layer 2 Ethernet frame and wraps it inside a regular Layer 4 UDP packet. Wrapping it this way lets Layer 2 traffic travel across Layer 3 networks it could not normally cross. (If the wrapping idea sounds familiar, it is the same principle as the [tunnelling I covered in my IP types post](https://fionnlinux.com/posts/ip-types-traffic-types/) — put one packet inside another so it can travel where it otherwise could not.)
- **Data center interconnect (DCI)** — the main use case. VXLAN lets physically separate data centres behave as one logical Layer 2 network, which is what makes large cloud environments possible.

## Zero Trust Architecture (ZTA)

The old security model trusted anything already inside the network perimeter. Get past the outer wall and you were assumed friendly. Zero Trust throws that assumption out. The principle is "never trust, always verify" — every request is authenticated and authorised regardless of where it comes from, inside or outside the network. There is no trusted inside.

The exam breaks it into three parts:

- **Policy-based authentication** — access decisions are driven by defined policies, not by location on the network. Who you are and what the policy allows, checked every time.
- **Authorization** — there are two separate checks. Authentication proves who you are. Authorization decides what you are allowed to do once you are in. Zero Trust treats them as distinct steps and checks both — proving your identity does not automatically grant you access to everything.
- **Least privilege access** — each user or system is given the minimum access needed to do its job, and nothing more. Less standing access means less damage if something is compromised.

In practice, Zero Trust thinking is the default-deny mindset: nothing is open unless it has been explicitly allowed, and access is granted narrowly rather than broadly. It describes how you build and configure a network, rather than a single product you can buy off the shelf.

## SASE and SSE

**SASE — Secure Access Service Edge** delivers networking and security together as a single cloud service, instead of as separate physical boxes on site. Rather than running a firewall, a secure web gateway, Zero Trust access and wide-area networking as individual appliances, SASE rolls them into one service delivered from the cloud. It combines ideas already in this post — SD-WAN for the networking, Zero Trust for the access.

**SSE — Security Service Edge** is the security half of SASE on its own, without the SD-WAN networking part. The simple way to hold the two apart: SSE is the security services; SASE is those same security services plus the networking.

## Infrastructure as Code (IaC)

This is the section I care most about, because it is the direction I am building toward.

Infrastructure as Code means defining your infrastructure — servers, networks, configurations — in text files rather than setting it up by hand. Instead of manually clicking through a cloud console or configuring a server by memory, you write the desired state in code, and a tool makes reality match it. The infrastructure becomes reproducible, reviewable, and version-controlled, exactly like software.

Two tools made this concrete for me, and the distinction between them is worth getting right because they are often mentioned together:

- **Terraform provisions.** It creates and sets up the infrastructure itself — the servers, the networks, the cloud resources. If you need ten virtual machines on Azure with the right networking around them, Terraform builds them from a definition file.
- **Ansible configures.** Once those machines exist, Ansible sets them up — installs packages, creates users, applies firewall rules, starts services. It configures what is already there.

The short version: Terraform builds the boxes, Ansible furnishes them. They are complementary, not competing.

### The automation side

The objective lists several automation concepts, and they are all reasons IaC is worth the effort:

- **Playbooks, templates and reusable tasks** — the files that describe what should be done. Written once, they can be reused across many machines. (In Ansible, a playbook is the file listing the tasks to run.)
- **Configuration drift and compliance** — over time, manually managed machines drift apart as small changes accumulate and nobody remembers exactly what was done where. Infrastructure as Code fixes this: the code is the single source of truth, and you can re-apply it to bring any machine back in line. That same property lets you prove a fleet is compliant with a required configuration.
- **Upgrades** — applying updates across many machines from one definition rather than touching each one.
- **Dynamic inventories** — the inventory is the list of machines your automation manages. A fixed list is written by hand, which works when your machines are stable. But in the cloud, servers are created and destroyed constantly, and a hand-written list would be out of date immediately. A dynamic inventory builds itself automatically by asking the cloud platform what machines currently exist, so the list is always accurate without you maintaining it.

### The source control side

This is the part that connects to how I already work day to day. Every change to this site goes through Git with structured commit messages, pushed to two separate hosts for redundancy. IaC depends on exactly this discipline, and the objective lists the pieces:

- **Version control** — every change to the infrastructure code is tracked. You can see what changed, when, and why, and roll back if something breaks. The infrastructure gets the same history and accountability as application code. This site itself works this way — every change to it is committed to a [public Git repository](https://github.com/fionnlinux/fionnlinux.com) with a clear message explaining what changed and why. You can see the full history there if you are curious.
- **Central repository** — the code lives in one shared place that a whole team works from, rather than scattered across individual machines.
- **Branching** — work on a change in isolation on its own branch, test it, then merge it into the main code once it is ready, without disturbing what is live.
- **Conflict identification** — when two people change the same thing, version control flags the conflict so it gets resolved deliberately rather than one change silently overwriting the other.

Getting properly proficient with Git, branching and CI/CD pipelines is one of the things I am working toward, because it is the backbone of how infrastructure is managed in any serious environment. It is not a side skill to IaC — it is the foundation underneath it.

What pulls all of this together for me is **bootc** and image-mode Linux: the idea of defining an entire operating system as code, building it from a configuration, and deploying it as a reproducible image. It is Infrastructure as Code applied to the OS itself, and it is the direction I want to contribute to once I am further along. The same principle running through this whole section — describe the desired state in code, let the tooling make it real — taken all the way down to the operating system.

## IPv6 Addressing

I first assumed IPv6 was only about having more addresses. That is the main driver, but not the whole story.

**Mitigating address exhaustion** is the headline reason. IPv4 uses 32-bit addresses, which gives about 4.3 billion — and the world ran out. IPv6 uses 128-bit addresses, which provides a number so large it removes the problem entirely for any foreseeable future.

Because IPv4 and IPv6 cannot talk to each other directly, and the whole internet cannot switch overnight, the objective focuses on **compatibility requirements** — the methods that let the two coexist during the long transition:

- **Dual stack** — a device runs both IPv4 and IPv6 at the same time, using whichever the other end supports. This is the most common approach.
- **Tunneling** — IPv6 traffic is wrapped inside IPv4 packets (or the reverse) so it can cross a network that only speaks the other protocol. The same encapsulation idea that appears throughout this post.
- **NAT64** — a translation mechanism that lets IPv6-only devices communicate with IPv4-only services by translating between the two.

Beyond the addressing, IPv6 brought other changes worth knowing: it does away with broadcast traffic (using multicast instead), has a simplified header design, and was built with IPSec support in mind from the start rather than bolted on later.

## Wrapping Up

That closes out Section 1. The thread running through every topic here is the same: networking is moving away from hand-configured physical boxes and toward software, automation and code. SDN moves control into software. SASE moves security into the cloud. Infrastructure as Code turns infrastructure into version-controlled text. Even IPv6 is the plumbing being modernised underneath.

Not that long ago I did not really understand what the cloud even was — it seemed like some sort of magic happening somewhere I could not see. Now I am studying it properly and getting to see the opportunities it opens up. There are several roles in this space that genuinely interest me, and being able to learn while the whole field is still moving and growing makes it a good time to be getting into it. That is why I am jumping in at the deep end.
