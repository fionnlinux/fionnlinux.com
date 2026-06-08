---
title: "IPv4 Addressing — Making the Numbers Make Sense"
date: 2026-06-08T00:00:00+01:00
draft: false
description: "Private addresses, loopback, address classes, and subnetting — explained through my home network, a Podman container, and the mental-maths method that finally made subnetting click."
tags: ["networking", "comptia", "security", "ipv4", "subnetting", "homelab"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

My understanding of IP addresses used to begin and end with a "what is my IP" website, or the occasional ping when something would not connect. They were just numbers that appeared when I needed them. I never thought about where they came from or what they actually meant.

That changed a few months ago, and oddly it started with my kids. I was studying for Network+ at the same time as setting up Mineclonia — an open-source, Minecraft-style game — for them to play and host at home. To get them connecting to the server, I had to start actually looking at the addresses on our home network, and I noticed something. Every device in the house had an address that looked almost identical: the same first three numbers, with only the last number different. To reach a particular machine — the game server, another computer, a device sharing files — I kept the first three numbers the same and changed the last one.

At the time I just took that as how things worked. It was only when I reached the IPv4 addressing and subnet mask section of my studies that I realised I had stumbled onto one of the core concepts of the whole objective without knowing it. I had been reasoning about a subnet — I just had no words for it, and no idea of the theory underneath.

This post is my attempt to explain IPv4 addressing in a way that would have helped me a few months ago — plainly, grounded in real things, and without pretending subnetting is easy when it is the part most people find genuinely difficult. By the end you should be able to look at an IP address and a subnet mask and actually understand what they are telling you.

## What an IP Address Actually Is

An IPv4 address is four numbers separated by dots, like this:

```
192.168.1.10
```

Each of those four numbers is called an **octet**, and each one can be anything from 0 to 255. That gives a range from `0.0.0.0` all the way up to `255.255.255.255`.

The address does two jobs at once, and this is the single most important idea in the whole topic: part of the address identifies the **network** a device is on, and part identifies the **specific device** on that network. Think of a postal address. "Baker Street" identifies the street; "221B" identifies the specific house on it. An IP address works the same way — some of it is the street, and some of it is the house number.

The obvious question is: which part is the street and which part is the house? That is exactly what the subnet mask tells you, and we will get there. But to understand the subnet mask, you need to peek at the layer underneath the numbers — binary. It is less frightening than it sounds, and a little of it makes everything else fall into place.

## Binary — Just Enough to Make Sense of It

Computers do not see `255`. They see eight switches, each either **on** or **off**. Eight switches is what one octet is, underneath. That is why the maximum is 255 and not 1,000 or anything rounder — 255 is simply the largest number eight on/off switches can represent.

Here is the part that makes it usable. Each switch has a value, and the values double as you move from right to left:

```
128   64   32   16   8   4   2   1
```

When a switch is **on**, you count its value. When it is **off**, you ignore it. Add up all the switches that are on, and that is your number.

- All eight off = 0
- All eight on = 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = **255**
- Just the first two on = 128 + 64 = **192**

That doubling sequence — 1, 2, 4, 8, 16, 32, 64, 128 — is worth burning into memory, because it is the backbone of everything that follows. Notice what happens when you switch the values on one at a time, starting from the left and keeping a running total:

```
128                       = 128
128 + 64                  = 192
128 + 64 + 32             = 224
128 + 64 + 32 + 16        = 240
128 + 64 + 32 + 16 + 8    = 248
... + 4                   = 252
... + 2                   = 254
... + 1                   = 255
```

So the sequence **128, 192, 224, 240, 248, 252, 254, 255** is just "turn on one more switch from the left each time." These are the only values that legitimately appear in a subnet mask, and now you can see exactly why. Every one of them is a running total of switches being turned on from the high end. You do not need to memorise the list as random numbers — you can rebuild it any time by doubling.

That is genuinely all the binary you need for this objective. Hold on to it.

## The Subnet Mask — Where the Street Ends and the House Begins

The subnet mask is a second set of four numbers that sits alongside the IP address and answers the question from earlier: which part of the address is the network (the street) and which part is the device (the house).

The most common subnet mask, and the one on my home network, is:

```
255.255.255.0
```

Remember that 255 means "all eight switches on" and 0 means "all eight switches off." A switch that is **on** in the mask means "this part is the network." A switch that is **off** means "this part is the device." So `255.255.255.0` says: the first three octets are the network, and the last octet is the device.

That is the theory behind what I was doing with the Mineclonia setup. On my home network the mask is `255.255.255.0`, so the first three numbers (`192.168.1`) are the street — the network everyone shares — and the last number is the individual house. To reach a different device, I keep the street the same and change the house number. I had been following the subnet mask without knowing it existed.

With the last octet free for devices, that octet can be anything from 0 to 255 — 256 possibilities. But two of those are always reserved: the first address (`192.168.1.0`) is the **network address**, a label for the network itself, and the last (`192.168.1.255`) is the **broadcast address**, used to talk to every device at once. Take those two away and you are left with **254 usable addresses** for actual devices. That is where the 254 comes from, and it is the first piece of subnetting maths.

## CIDR Notation — The Slash Number

Writing out `255.255.255.0` every time is tedious, so there is a shorthand called **CIDR notation** (Classless Inter-Domain Routing). You will see it everywhere, and it looks like this:

```
192.168.1.0/24
```

The number after the slash is simply **how many switches are on in the mask**, counting from the left. A mask of `255.255.255.0` has 24 switches on (three octets of eight), so it is written `/24`. That is all the slash means: the count of network switches.

This matters because CIDR is the modern way networks are actually described. The older system, which we will look at next, locked you into fixed sizes. CIDR lets a network be any size you like by simply moving where the network part ends — turning one more switch on or off. A `/25` has one more network switch than a `/24`; a `/23` has one fewer. Once you are comfortable with the slash number being a switch count, CIDR stops being intimidating.

## Public vs Private Addresses

Here is something you can check right now. The address of your device on your home network almost certainly starts with `192.168`. So does mine. So does nearly everyone's. If these addresses are supposed to be unique, how can millions of separate homes all be using `192.168.1.x` at the same time?

The answer is that some address ranges are deliberately set aside as **private**. They are reserved for use inside local networks and are never used directly on the public internet. Because they never appear on the public internet, the same private range can be reused in every home, office, and network in the world without conflict. These ranges are defined in a standard called **RFC1918**, and there are three of them:

```
10.0.0.0    to 10.255.255.255    (a huge range, used by large organisations)
172.16.0.0  to 172.31.255.255    (a mid-sized range)
192.168.0.0 to 192.168.255.255   (the small range home routers use)
```

Your home router hands out addresses from that last range to everything in the house. When your traffic needs to reach the actual internet, the router translates your private address into the single **public** address your internet provider gave you — a process called NAT (Network Address Translation), which is a topic of its own. The short version: private addresses work inside your network, public addresses work on the open internet, and your router sits on the boundary translating between the two.

This is also why the private-versus-public distinction matters for security. A private address cannot be reached directly from the internet — it sits behind the router, out of view, and on its own it is meaningless to anyone outside your network. That protection comes for free, simply from how private addressing works.

It is worth being clear about what that protection does and does not cover, though. It stops direct access from outside. It does not help against a threat that is already inside your network — a compromised device, or someone sitting on the same WiFi, can use private addresses to reach other machines on that network. And it can be undone by accident: if you configure your router to expose a private device to the internet — opening up a home server or camera without securing it first — that device becomes reachable through your public address, and the free protection is gone. Private addressing is a genuine first layer, not a wall.

## Two Special Addresses Worth Knowing

A couple of specific addresses come up constantly, both in the exam and in real work.

**Loopback — 127.0.0.1.** This address always means "this very device, talking to itself." It never leaves the machine. I ran into this properly while learning containers with Podman. When I looked up the secure way to run a service, the advice was repeatedly to **bind it to 127.0.0.1** — also called **localhost**. Doing that means the service can only be reached from the machine it is running on. Nothing else on the network, and nobody on any other machine, can touch it. That was my way into understanding loopback: not as something to memorise for an exam, but as a deliberate security choice. Binding to loopback is one of the simplest ways to keep something private to a single host.

**APIPA — 169.254.x.x.** Normally a device gets its address automatically from the router (via DHCP, another topic of its own). But if a device asks for an address and gets no answer — the router is down, the DHCP service has failed, a cable is unplugged — the device gives up waiting and assigns itself an address in the `169.254` range. This is **APIPA** (Automatic Private IP Addressing). The practical thing to remember is what it signals: if you ever see a device sitting on a `169.254` address, it almost always means it failed to reach the DHCP server. It is less a useful address and more a symptom — a flashing warning light that automatic addressing has broken somewhere.

## IPv4 Address Classes

Before CIDR existed, addresses were divided into fixed **classes**, decided by the value of the first octet. The classful system is largely historical now — CIDR replaced it precisely because fixed classes were wasteful — but it is still on the exam and still referenced in the field, so it is worth knowing.

There are five classes, A through E, and you can identify them purely by the first number of the address:

```
Class A   1   – 126     Default mask 255.0.0.0   (/8)    Very large networks
Class B   128 – 191     Default mask 255.255.0.0 (/16)   Medium networks
Class C   192 – 223     Default mask 255.255.255.0 (/24) Small networks
Class D   224 – 239     No mask — used for multicast
Class E   240 – 255     No mask — reserved/experimental
```

A few things to take from that table. Class A networks are enormous, which is why only the largest organisations ever held one. Class C is the small-network size — and notice that its default mask is `255.255.255.0`, the same `/24` my home network uses, and the `192.168` private range falls inside Class C territory. **Class D** is set aside for **multicast**, which is one-to-many traffic (a single stream sent to many devices at once). **Class E** is **reserved and experimental** — you will not use it, but you may be asked to recognise it.

You might also notice `127` is missing from the list. That whole range is reserved for the loopback address we just covered, which is why Class A stops at 126.

**An easier way to remember the classes.** You do not need to memorise five separate ranges. Look only at where each class *starts*: A at 1, B at 128, C at 192, D at 224, E at 240. Those starting points — 128, 192, 224, 240 — are the very same doubling sequence from the binary section earlier. You already know them, so the class boundaries come for free. And the three you will actually use are the simplest part of all: A begins at 1, B at 128, C at 192. D and E are just the leftovers at the top — D for multicast, E reserved.

## Subnetting — Carving One Network Into Several

This is the part most people find hardest, and it is the part I have found hardest too — it is still the section I slow down and think through rather than answer on instinct. But the method I use is mental maths, not long-winded binary on paper, and I think it is the most approachable way in.

**Why subnet at all?** A single network with 254 addresses might be far more than one part of an organisation needs, and putting everything on one flat network is both wasteful and, as I wrote about in my post on network topology, a security problem — a flat network lets a threat move freely. Subnetting is how you carve one network into smaller, separate pieces, by **borrowing switches** from the device part of the address and giving them to the network part — turning the slash number up from `/24` to `/25`, `/26`, and so on.

One honest clarification, though, because it caught me out: subnetting on its own only *draws* the boundaries. It does not enforce them. What actually stops traffic crossing from one piece to another is the rules you place between them — firewall policies, access control lists, or VLANs. Subnetting is the structure that real segmentation is built on; those controls are what make the separation stick. Drawing different subnets on a flat home network, with nothing enforcing the split, is lines on a map rather than walls — which is exactly the distinction I will be getting hands-on with when I build the OPNsense setup.

**The method.** When a subnet mask has an octet that is not a clean 0 or 255 — say `255.255.255.192` — that octet is the one doing the work. The trick is one subtraction:

```
256 − 192 = 64
```

That **64** is your **block size** — the number of addresses in each subnet, and the gap between where one subnet starts and the next begins. With a block size of 64, the subnets fall at:

```
192.168.1.0     (covers .0   to .63)
192.168.1.64    (covers .64  to .127)
192.168.1.128   (covers .128 to .191)
192.168.1.192   (covers .192 to .255)
```

Four subnets, each holding 64 addresses. And just like before, each subnet loses two addresses to its own network and broadcast address, so each one has **62 usable** addresses (64 − 2).

That is the whole method in one line: **256 minus the interesting octet gives you the block size, and you count up in blocks of that size.** Remember the doubling sequence from the binary section — `128, 192, 224, 240, 248, 252, 254` — those are the only values that interesting octet can be, and each one gives a different block size when you subtract it from 256. A larger mask value means a smaller block, which means more subnets with fewer hosts in each. It is all the same trade-off, dialled up or down.

**VLSM.** One last concept in the objective: **Variable Length Subnet Masking**. In the old classful world, every subnet had to be the same size. That wasted enormous numbers of addresses — a link between two routers needs only two addresses, but you would have been forced to give it the same large block as a department of fifty people. VLSM means using **different mask sizes for different subnets**, sizing each one to what it actually needs. Big subnet for the big department, a tiny `/30` (just two usable addresses) for the router link. It is simply the sensible idea that not every piece of a network is the same size, so the masks should not be either.

## How I Am Actually Learning This

Honestly, this has been the hardest part of the exam objectives for me so far. It has taken more time than I would like, and I am not going to pretend the maths falls out of my head instantly yet — it does not.

What helps is practice, plain and simple. Working through questions and drilling on a subnetting practice site — the kind that throws an address and a mask at you and asks for the network, the broadcast, the usable range, and the host count — is slowly turning it into muscle memory. I have linked the one I use on my [resources page](/resources/). There is no clever shortcut: you understand the method once, then repeat it until your brain stops having to work for the answer.

So if you are studying the same thing and finding this objective a slog, you are in good company. Read this once to understand *why* the maths works, then go and drill it until the why fades into the background and the answers simply come.

## Why This Matters Beyond the Exam

The best part of learning this has been watching the numbers turn into things I recognise. Those four little numbers are the reason every device in my house shares the first three and differs on the fourth. They are why binding a game server or a container to `127.0.0.1` locks it to one machine where nobody else can poke at it. They are why a stray `169.254` address is a dead giveaway that something has fallen over, before you have even opened a log. And subnetting — the part that felt like a wall at first — is what lets you carve a network into separate zones in the first place: the groundwork that real segmentation, with its firewall rules and VLANs, is built on top of.

What I enjoy is how ordinary it all turns out to be once you understand it. I started by changing the last number of an address to get my kids onto their Mineclonia server, with no clue why it worked. Now I know exactly why — and that small win is the bit I would hold on to if you are just starting out. A few months ago these were just numbers on a screen. Now they tell me a story, and there is a quiet contentment in watching something that once felt completely foreign slowly fall into place. No matter how slow that is.
