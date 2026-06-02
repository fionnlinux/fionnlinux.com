---
title: "TCP/IP — The Acronym I Ignored, and Why SOC Roles Demand It"
date: 2026-06-01T00:00:00+01:00
draft: false
description: "TCP/IP shows up in every SOC job posting and every networking exam. Here is what it actually means — explained through a relative's website, a Podman container, and a twenty-year-old memory of a game server going down."
tags: ["networking", "security", "tcp-ip", "comptia", "soc", "homelab", "personal"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

When I first started studying CompTIA certifications, TCP/IP was just an acronym. I had come across IP addresses before — everyone has — but TCP meant nothing to me beyond the letters. Like a lot of the networking world, it looked like abbreviations floating in space with nothing underneath them — terms I could half-recite from a study guide but could not actually connect to anything real. That gap used to frustrate me. I could read the words but they did not attach to anything.

It was only recently, working through CompTIA Network+ and seeing "TCP/IP protocol understanding" listed in nearly every entry-level SOC analyst job posting I looked at, that I decided to properly understand it rather than just memorise it for an exam. And the more I dug in, the more I realised something strange — I had been watching TCP/IP work, and watching it break, since I was a teenager. I just had no idea that was what I was looking at.

## What Finally Made It Click

Before any of the theory landed, I needed something to attach it to. Three things gave me that.

The first was building a website for a relative and having to configure how it connected to the world. The second was learning containers with Podman, where I kept running into the need to allow specific ports through before a container could communicate. Suddenly ports were not an abstract exam term — they were the thing standing between my container working and not working. The third was seeing the same protocols named over and over in SOC job descriptions, which told me this was not academic. It was the foundation of the work I am aiming for.

I still have to think about it. It is not yet instinct. But I have reference points now, and that changes everything. So this is my attempt to explain TCP/IP the way I wish someone had explained it to me — by connecting it to something real.

## What TCP/IP Actually Is

The first thing that helped was realising TCP/IP is not a single thing. It is a suite — a whole family of protocols that together make the internet work. People say "TCP/IP" as shorthand for the entire system, not one protocol.

It is often described as a four-layer model, which maps roughly onto the seven-layer OSI model used in study material:

```
TCP/IP Model       OSI Equivalent
Application    →   Layers 5, 6, 7
Transport      →   Layer 4
Internet       →   Layer 3
Network Access →   Layers 1, 2
```

The OSI model is the teaching tool — seven neat layers to help you reason about where things happen. TCP/IP is what the internet actually runs on. CompTIA teaches both because you need the OSI model to understand the concepts and TCP/IP to understand reality.

For this post the two layers that matter most are the Internet layer, where IP lives, and the Transport layer, where TCP and UDP live.

## IP — Addresses and Directions

IP, the Internet Protocol, is the part that handles addressing and routing. Every device on a network has an IP address, and IP is responsible for getting a packet of data from one address to another across however many networks sit in between.

The simplest way I found to think about it is a road network. An IP address is a destination — a specific building you are trying to reach. IP is the system of roads and junctions that gets a vehicle from one address to another, choosing a route across whatever networks sit in between. It does not guarantee the vehicle arrives, it does not care what is being carried, and it does not check whether anything got lost on the way. It just handles addressing and direction.

That last point matters, because IP on its own is not reliable. It does its best to deliver, but it makes no promises. Something has to sit on top of IP to decide whether reliability matters — whether a lost delivery needs noticing and fixing, or whether it can simply be let go. That something is TCP or UDP.

## TCP — The Delivery You Can Count On

TCP, the Transmission Control Protocol, is the reliable one. It is connection-oriented, which means before any real data is sent, the two devices establish a proper connection and agree they are both ready. TCP guarantees that data arrives, arrives in order, and arrives intact. If a packet goes missing, TCP notices and resends it.

That guarantee starts with the three-way handshake — the single most important thing in this whole post, and the thing that appears constantly in SOC work.

It goes like this:

- **SYN** — the client sends a synchronise packet. "I want to connect."
- **SYN-ACK** — the server replies with synchronise-acknowledge. "I heard you, I am ready, are you?"
- **ACK** — the client sends acknowledge. "Confirmed. Let's go."

Three steps, and the connection is established. Only then does real data flow. The analogy that made it stick for me is a phone call — you say hello, the other person says hello back, you confirm you can hear each other, and only then do you start the actual conversation. The handshake is that mutual "can you hear me" before anything important is said.

When the conversation is over, TCP closes the connection cleanly too, with a similar exchange of FIN and ACK packets — essentially both sides agreeing to hang up rather than just dropping the line.

Those little flags — SYN, ACK, FIN, and a few others like RST (reset, used to abruptly kill a connection) and PSH (push) — are worth knowing by name. A SOC analyst reads these constantly. They are the vocabulary of what a connection is doing, and whether what it is doing is normal.

TCP is what you want when delivery has to be correct — loading a web page over HTTPS, logging in over SSH, sending email. Anything where a missing piece would break the whole thing.

## UDP — Fast and Forgetful

UDP, the User Datagram Protocol, is the opposite trade-off. It is connectionless — no handshake, no setup, no guarantee. You send the data and hope it arrives. If a packet goes missing, UDP does not notice and does not care.

That sounds worse, and for some things it would be. But for others it is exactly right. If you are on a video call and one packet of audio drops, you do not want the call to stop and wait for that packet to be resent — by the time it arrived the moment would have passed. You want it to carry on and stay live. Streaming, online gaming, voice calls, and DNS lookups all use UDP because speed and staying current matter more than perfect delivery.

The way I hold the difference in my head: TCP is the postal service with tracking and a signature required. UDP is dropping a postcard in the box and trusting it turns up. Neither is better. They are different tools for different jobs, and choosing the wrong one would make either job worse.

## The Part I Had Already Seen Without Knowing It

Here is where it got personal, and where the theory suddenly connected to something twenty years old.

As I have written about before, my teenage years were spent playing Tibia, an online MMORPG. Two things happened in that world regularly that I never understood at the time, and both turn out to be textbook TCP/IP.

The first was the servers going down. Every so often the game would become unplayable — servers crashing, everyone knocked offline. The word going round was always that someone was "DDoSing" the servers. I had no idea what that meant. Now I do. A distributed denial of service attack floods a server with more traffic than it can handle until it collapses. One common form is a SYN flood — an attacker sends thousands of SYN packets, the first step of that three-way handshake, but never sends the final ACK to complete it. The server holds all those half-open connections open, waiting for replies that never come, until its resources are exhausted and it cannot serve anyone. I had watched that happen to a game I played for hours without understanding a single mechanic of it.

The second was darker and more targeted. Tibia involved a lot of voice chat over Ventrilo while hunting or fighting other players. People learned that if they could get someone's IP address from the voice server, they could attack that individual's home connection directly — flooding it with traffic so the person would lag out at a critical moment, frozen in place during a dangerous hunt or a player-versus-player battle, and get killed in the game because their character stopped responding. That is not quite a DDoS in the distributed sense — it is a denial of service aimed at one person's connection — but it is the same fundamental idea. Overwhelm a target with traffic until it cannot function. An IP address harvested from a voice server someone had willingly joined, then weaponised against them at the worst possible moment. I saw it happen to people I played alongside. I never once understood how.

Understanding it now, two decades later, is oddly satisfying. The same game that first put me in front of a computer turns out to have been my first exposure to network attacks. I just needed twenty years and a Network+ textbook to realise what I had been looking at.

## What This Looks Like From a Security Seat

I want to be honest about where I am. I have watched people dissect packets in Wireshark on YouTube, and seeing a capture broken open visually is what made the handshake real for me rather than abstract. But I have not yet sat in front of live traffic doing it myself. That comes as my homelab grows, and it will get its own posts when it does. What I can explain is the theory of what to look for and why it matters — the foundation that makes the practical work make sense when I get there.

From a security perspective, these protocols are where a lot of malicious activity becomes visible:

- **SYN floods** — the Tibia server attack. Huge volumes of half-open TCP connections, handshakes started and never finished, designed to exhaust a server.
- **Port scanning** — tools like Nmap probe a target to see which ports respond. Often this is done with a deliberately incomplete handshake — the scanner sends a SYN, sees whether the port replies, but never completes the connection, which is both faster and quieter than a full connection. In logs it shows up as one source making rapid connection attempts across many ports, a classic reconnaissance signature. That deliberate half-open behaviour is the same mechanic a SYN flood abuses, just used for a different purpose.
- **UDP floods** — overwhelming a target with UDP traffic. No handshake pattern, just raw volume, which is a different signature to a SYN flood.
- **Unexpected RST packets** — abrupt connection resets that can indicate scanning, a firewall actively refusing connections, or something more hostile depending on context.

Here is one scenario that makes the potential attacks real for me. Imagine a SIEM flagging login attempts on a work machine from an IP address in another country, when you know the person who uses that machine is still in the same town as you. Understanding TCP/IP is what lets you look at that and understand what the connection actually is, where it came from, and why it is a red flag. None of it makes sense without the foundation underneath.

The crucial point is that you can only spot abnormal traffic if you know what normal looks like. A completed three-way handshake is normal. Ten thousand half-open ones from a single source is not. The protocols are the baseline against which everything suspicious is measured.

## Why It Is in Every SOC Job Posting

When I first saw "TCP/IP protocol understanding" in a job listing it meant almost nothing to me. Now I understand what it is really asking. You cannot investigate an alert, read a firewall log, or make sense of what a SIEM is telling you without understanding what the underlying protocols are doing. Every signature, every alert, every packet capture traces back to these fundamentals. A SOC analyst who does not understand TCP/IP is reading a language without knowing the grammar.

I am still building that fluency. But I am no longer staring at an acronym with nothing behind it. I have reference points now — a relative's website, a Podman container, and a twenty-year-old memory of a game server crashing for reasons I am only now equipped to understand.

That is the difference between studying something and actually learning it. The letters were always there. Understanding what they meant took a lot longer — and a surprisingly vivid set of memories from a game I played twenty years ago.
