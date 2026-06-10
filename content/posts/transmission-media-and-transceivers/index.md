---
title: "Transmission Media — From Copper I've Pulled to the Wi-Fi I Got Backwards"
date: 2026-06-09T00:00:00+01:00
draft: false
description: "Cables, fibre, wireless standards, transceivers, and connectors — the physical layer that carries everything, explained with the memory aids I wish I'd had, including the Wi-Fi frequency trade-off I had completely backwards."
tags: ["networking", "comptia", "security", "wireless", "cabling"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

When I set up the Wi-Fi in our house, I made a confident decision based on something I had completely wrong. Our whole home runs on wireless — every device, including my kids' desktop tower, which did not have a wireless card, so I added a USB wireless adapter to get it online. When I was deciding which devices should sit on which band, I told myself that 5 GHz was the better choice for the rooms further from the router, because I believed 5 GHz was better at getting through walls.

It is the opposite — and I only realised when I double-checked myself against the Network+ objectives. This whole section is one I keep forgetting, because so much of it comes down to remembering numbers and names, and the frequency trade-off was one more thing I had filed away the wrong way round.

This objective covers transmission media — the physical stuff that actually carries data, whether that is copper cable, glass fibre, or radio waves through the air — along with the transceivers and connectors that join it all together. It is a big objective with a lot of small facts, and it is exactly the kind of thing that blurs together if you try to brute-force memorise it. So this post is my attempt to make it stick, built around the things I have actually handled, and the memory aids that are slowly getting the rest to stay put.

## Wireless — The Frequency Trade-Off

Frequency was not a totally new idea to me coming into this. As an electrician I already knew that mains electricity runs at a different frequency depending on where you are — the UK runs at 50 Hz, the US at 60 Hz — something I had to get my head around when I moved from the UK to the States. So the idea of frequency mattering was familiar. What was new was how it behaves over the air.

Wireless networks run on different frequency bands — mainly **2.4 GHz** and **5 GHz**, with **6 GHz** added in the newest generation. The single most important thing to understand is the trade-off between them:

```
Lower frequency (2.4 GHz)  →  slower, but longer range and better through walls
Higher frequency (5 GHz)   →  faster, but shorter range and worse through walls
Highest (6 GHz)            →  fastest, shortest range, newest
```

Here is the way I finally got it to stick, and it is the actual physics rather than a trick. It comes down to the size of the wave. A lower frequency like 2.4 GHz has a longer wave, and longer waves are better at passing through and bending around solid objects like walls — but they carry less data. A higher frequency like 5 GHz has a shorter wave that carries far more data, but those shorter waves are blocked and absorbed by walls much more easily, and they weaken faster over distance.

If you want something familiar to hang it on, think of distant thunder. From far away, or through a closed window, you hear the low rumble — never the sharp high crack, which only reaches you when the storm is close. Low frequencies travel far and get through; high frequencies are stopped short. Wi-Fi behaves the same way: 2.4 GHz is the low rumble that reaches the far rooms, and 5 GHz is the sharp detail that needs to be close to come through clearly.

So my instinct was exactly backwards. For the rooms far from the router, 2.4 GHz is actually the more reliable choice, because it reaches further and copes better with walls in the way. 5 GHz is the band you want when the device is close to the router and you care about speed. Once the thunder picture is in your head, it is hard to get the wrong way round again.

There is a real-world cost to 2.4 GHz worth knowing too: it is crowded. Microwaves, Bluetooth devices, baby monitors, and every neighbour's older router all sit on 2.4 GHz, so it suffers far more interference. 5 GHz is less congested and has more channels to spread across. That congestion is part of why 5 GHz often feels faster in practice, on top of its higher raw speed.

## Wireless — The 802.11 Standards

I will be honest: this is the part of the whole objective I have found hardest, and I am not going to pretend otherwise. The Wi-Fi standards are all named **802.11** with a letter on the end — a, b, g, n, ac, ax — and they do not run in obvious alphabetical order, the bands change between them, and the specs blur together. For me this is pure memorisation. It has not fully gone in yet, and writing it out like this is part of how I am trying to force it to.

The one thing that is helping is that the standards you most need to know map directly onto the friendly **Wi-Fi 4, 5, and 6** names that marketing now puts on the box:

```
Standard   Marketing name   Band(s)            Rough max speed
802.11a    —                5 GHz              54 Mbps
802.11b    —                2.4 GHz            11 Mbps
802.11g    —                2.4 GHz            54 Mbps
802.11n    Wi-Fi 4          2.4 and 5 GHz      600 Mbps
802.11ac   Wi-Fi 5          5 GHz              ~7 Gbps
802.11ax   Wi-Fi 6 / 6E     2.4, 5, (6) GHz    ~9.6 Gbps
```

A few anchors I am using to hold it together. The oldest pair, **a and b**, arrived around the same time — and the way I keep them straight is that **a** ran on the higher band (5 GHz) and was faster, while **b** ran on 2.4 GHz and was slower. **g** then brought the higher speed down onto the 2.4 GHz band. **n** (Wi-Fi 4) was the big jump that introduced using both bands and multiple antennas. After that it is just: **ac is Wi-Fi 5, ax is Wi-Fi 6.** If I can hold on to "n, ac, ax = Wi-Fi 4, 5, 6," I have the modern ones, which are the ones that come up most.

One thing worth saying about the speeds in that table: they are the best-case numbers on paper. In the real world you never actually hit them — real speeds are always lower than the headline figure. For the exam it matters more to know the order of the standards, which bands they use, and which generation is which than to recall the exact number.

## Wireless — Cellular and Satellite

Two other wireless media appear in the objective, both used to connect over much larger distances than Wi-Fi.

**Cellular** is the mobile-phone network — 4G LTE and 5G. In networking terms it is used for connectivity where running a cable is impractical: a remote site, a temporary location, a vehicle, or as a backup link that takes over if a site's main wired connection fails.

**Satellite** sends the signal up to an orbiting satellite and back down. Its historic weakness is **latency** — traditional satellites sit so far away that the round trip introduces a noticeable delay, which is poor for anything real-time. Newer low-orbit services (the kind Starlink uses) sit much closer to Earth and cut that latency dramatically. Satellite's role is reaching places nothing else can — rural and remote areas with no cable and no cellular coverage.

## Wired — Copper and the 802.3 Standards

Now onto the media I already knew with my hands before I knew the theory. As an electrician I pulled a lot of twisted-pair copper cable through buildings and terminated it onto **RJ45** connectors — the standard 8-pin plug on the end of every Ethernet cable.

Wired Ethernet is defined by the **802.3** family of standards, and for copper the practical thing to know is the cable categories and the speeds they support:

```
Category   Speed        Notes
Cat 5      100 Mbps     Older, now obsolete for new installs
Cat 5e     1 Gbps       "e" for enhanced; the common baseline
Cat 6      1–10 Gbps    10 Gbps only over short runs
Cat 6a     10 Gbps      "a" for augmented; 10 Gbps over full distance
```

The pattern is the thing to remember: as the category number goes up, the supported speed goes up. Standard copper runs have a maximum length of **100 metres** before the signal degrades too far — a number worth knowing because it comes up constantly, and it is the kind of practical limit I had to respect on the job without knowing it had a formal spec behind it.

## Wired — Fibre Optic

Fibre carries data as pulses of light through glass rather than electrical signals through copper. It travels much further and much faster, and it is immune to electromagnetic interference. There are two types, and the difference between them is simpler than it first sounds:

**Single-mode fibre** has a tiny core that lets light travel in a **single** path, fired by a laser. One clean path means the signal stays coherent over very long distances — think between buildings, across a city, long-haul links.

**Multimode fibre** has a wider core that lets light travel in **multiple** paths at once, usually from a cheaper LED-type source. Those multiple paths spread out slightly over distance and blur the signal, so multimode is limited to shorter runs — within a building or a data centre.

The memory aid is in the names themselves: **Single-mode = Single path = long distance. Multimode = Multiple paths = short distance.** Single-mode costs more and goes further; multimode is cheaper and stays local.

## Wired — Coaxial and Direct Attach Copper

Two more copper media round out the wired side.

**Coaxial cable** is the thick, single-core shielded cable you will recognise from cable TV and cable internet. In networking it is mostly legacy now, but it still appears on the exam and still carries broadband into homes. Its connectors are the screw-on **F-type** (cable TV and modems) and the older twist-lock **BNC**.

**Direct Attach Copper (DAC)** is a short, fixed copper cable with the transceivers already built onto each end. It uses **twinaxial** cable and is used inside data centres to link switches and servers that sit close together — for example connecting a server to the switch at the top of the same rack. It is cheaper and uses less power than fibre, but only works over very short distances, which is exactly what a rack needs.

## Plenum vs Non-Plenum — The Fire-Safety One

This is the part of the objective that lands closest to my trade background. **Plenum** refers to the air-handling spaces in a building — above a drop ceiling or under a raised floor, where air circulates for heating and cooling.

Cable run through those spaces has to be **plenum-rated**, meaning it has a special fire-resistant jacket that does not give off thick toxic smoke if it burns. The reason is straightforward: if a fire starts, those air spaces would otherwise pump poisonous fumes straight through the whole building's ventilation. **Non-plenum** cable (cheaper, standard jacket) is fine for ordinary walls and open runs but is not permitted in plenum spaces by fire code.

I worked with rules like this as an electrician, even if I did not know the networking term for it at the time. So this is one of the few parts of the objective I did not have to memorise from scratch — I already understood why the rule exists.

## Transceivers — The Swappable Port Modules

A **transceiver** is a small module that plugs into a switch or router and provides the actual physical connection — converting the device's internal signals into whatever needs to travel down the cable, whether that is electrical over copper or light over fibre. The point of them is flexibility: instead of a switch being hardwired for one cable type, you slot in the transceiver that matches the media and distance you need for that port.

Two form factors are named in the objective:

- **SFP (Small Form-factor Pluggable)** — the common smaller module, typically for 1 Gbps links (and SFP+ for 10 Gbps).
- **QSFP (Quad Small Form-factor Pluggable)** — a larger, higher-capacity module for much faster links, 40 Gbps and up. The "Quad" is the clue: it bundles four channels together, which is why it is faster.

Transceivers can also run different **protocols** — most commonly **Ethernet** (standard networking), but also **Fibre Channel (FC)**, which is a protocol built specifically for storage networks (connecting servers to storage arrays in a SAN).

## Connectors — A Quick Reference

Finally, the connectors. There is a long list, so here they are grouped by what they connect, with a memory hook where one exists:

```
Twisted-pair copper
  RJ45   8-pin Ethernet connector — the standard network plug
  RJ11   Smaller telephone connector — fewer pins, phone lines

Coaxial
  F-type  Screw-on, cable TV and cable internet
  BNC     Bayonet twist-lock, older coax networks and video

Fibre
  LC   Local Connector — small, the modern data-centre default
  SC   Subscriber Connector — square push-on body
  ST   Straight Tip — round with a bayonet twist lock
  MPO  Multi-fibre Push On — many fibres in one connector, for high-density 40/100G links
```

Those official names are the ones the exam uses, but the way I actually tell them apart is a set of unofficial nicknames: LC is the **Little** one, SC is the **Square** one, and ST is **Stab and Twist**, after its bayonet action. I have not handled enough of these connectors for them to feel familiar yet, and those three nicknames are the only thing keeping LC, SC, and ST from swapping places in my head.

## Why This Objective Is Worth the Effort

This is the most fact-heavy objective I have hit so far — the standards, the speeds, the connector names are all still a work in progress for me. Writing it out like this is how I am hammering it in.

What helps is that a good chunk of it is physically real, and some of it I had already handled. The copper I pulled and terminated, the 100-metre limit I worked within, the fire-rated cable I had to use in the right places — that was objective 1.5, years before I knew it had a number. Those parts I do not have to force, because I understand why they are true rather than just that they are.

The wireless side is the part I am still drilling, and the real value of it was being made to correct something I genuinely believed and had acted on. I set up my home network thinking 5 GHz reached further through walls, and it does not.

That mistake cost me nothing but time. But understanding why it was wrong, rather than guessing my way through, changed how I approached the rest of the objective. Now when I hit something unfamiliar, I pause and verify instead of carrying an assumption forward — and that habit will serve me far longer than any memorised number ever could.
