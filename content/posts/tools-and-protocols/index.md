---
title: "The Right Tool for the Job — Command Line, Software, and Hardware Testers"
date: 2026-07-26T00:00:00+01:00
draft: false
description: "A single connectivity complaint worked through with the actual tools this objective covers, from ping and traceroute to a cable tester, matching each one to what it can and cannot tell you."
tags: ["networking", "comptia", "troubleshooting"]
series: ["CompTIA Network+"]
series_order: 31
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

There is a specific image I have had in my head since I was a kid, of Boris — the villain's computer guy in GoldenEye — hammering away at a terminal, green text scrolling past, clearly pleased with himself. Not exactly a role model, but the image stuck. I still get a small version of that feeling every time I open a terminal myself, even for something as simple as `ping`. This final objective is basically a list of everything that feeling is actually built on — the real tools behind it, and specifically which one you would reach for first depending on what is actually wrong.

Picture a single complaint: a workstation on an office network cannot reach a specific internal server, though everything else it tries works fine.

## Starting With the Command Line

The command line is the fastest place to start, since most of these tools are already installed and take seconds to run.

**`ping`** is the very first check — sending a small test message to the server and waiting for a reply. In this case, it gets no response at all. That confirms something is wrong, but not yet what.

**`traceroute`** (`tracert` on Windows) goes further, showing every hop the traffic passes through on its way to that server. Run against the same destination, it shows the traffic reaching the office router and then stopping — nothing beyond that point. So the fault sits somewhere past the local network, not on the workstation itself.

Before going further, **`ipconfig`** (or `ifconfig`/`ip` on Linux) is worth checking on the workstation itself, just to rule out something simple — confirming it actually has a valid IP address and the correct gateway configured. Both check out fine, which rules out the workstation's own configuration as the cause.

**`arp`** is checked next, displaying the local table mapping IP addresses to MAC addresses. If the server's entry looked wrong, that would point toward something like ARP poisoning, covered in an earlier post. Here it looks normal, which rules that out too.

**`nslookup`** confirms the server's name is resolving to the correct IP address, ruling out DNS as the cause — the workstation is trying to reach the right address, it just cannot get a reply from it.

## Moving to the Server and Switch

With the workstation itself cleared, the next place to check is the server side and the network hardware between them.

**`netstat`**, run on the server itself, shows whether the expected service is actually listening on the right port at all. In this case, it is not listening — the service that should be running on that server has actually stopped.

**`show interface`**, checked on the switch the server connects through, confirms the physical port itself is up and passing traffic normally, which rules out anything like the cabling or interface faults covered in an earlier post. **`show config`** on the same switch confirms nothing was recently changed there either.

## What the Software and Hardware Tools Would Have Added

In this particular case, the fault turned out to be sitted entirely at the server — a stopped service, not a network problem at all — so the remaining categories of tool were not actually needed to reach the answer. But it is worth knowing where each one would have come in in a different scenario.

A **protocol analyzer** doing packet capture would have shown, frame by frame, whether the workstation's requests were even leaving the network correctly, useful if the command line results had been more ambiguous. A **port scanner** run against the server from elsewhere on the network could have confirmed the same "nothing listening on that port" result netstat gave, without needing access to the server itself. A **bandwidth speed tester** or **WiFi analyzer** would matter for a performance complaint rather than a total connection failure like this one.

If the fault had instead pointed toward the physical cabling rather than the server, a **cable tester** would confirm whether the cable itself was sound, a **tone generator** would help physically trace a specific cable through a bundle of others, and a **TDR** would locate a break at a specific distance along a copper run without needing to see it. None of those were relevant here, because the command line tools had already ruled out anything physical before hardware ever needed to be considered.

## Why the Order Mattered

Command line tools came first because they are fast and already available, and they managed to rule out the workstation, the local network, DNS, and ARP within a couple of minutes. Only once those were cleared did it make sense to look at the server and switch directly, and only if those had come back unclear would packet capture or physical hardware tools have actually been needed. Reaching for a cable tester before checking whether the service was even running would have wasted time solving a problem that was never physical to begin with.

That is also, as far as this whole series is concerned, the last piece of the objectives themselves. Everything from the OSI model right through to this list of tools has been building toward the same practical skill: recognising a symptom, knowing which layer or component it actually points to, and reaching for the right way to check it.
