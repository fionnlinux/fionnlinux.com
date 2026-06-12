---
title: "IP Types and Traffic Types — Filling In the Gaps"
date: 2026-06-11T00:00:00+01:00
draft: false
description: "ICMP, GRE, IPSec and the four traffic types — the smaller networking topics I had skipped over, learned well enough to recognise and explain."
tags: ["networking", "comptia", "protocols"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Not every networking topic gets its own headline. Some sit quietly in the objectives, easy to skim past when TCP/IP and subnetting are taking up most of the room. The IP protocol types and traffic types are exactly that — not complicated, but worth actually understanding rather than half-recognising.

TCP and UDP are covered in my [TCP/IP post](https://fionnlinux.com/posts/tcp-ip-the-acronym-i-ignored/), so I am not repeating them here.

## IP Protocol Types

TCP and UDP carry most everyday traffic, but they are not the only protocols that run over IP. Three more are worth knowing.

### ICMP — Internet Control Message Protocol

ICMP is the protocol the network uses to send status and error messages about itself. It does not carry your actual data — it reports on the network's condition.

The everyday example is **ping**. When you ping a device, your machine sends an ICMP request and waits for a reply. If it comes back, the device is reachable, and you can see how long the round trip took. **Traceroute** uses ICMP too, to show each hop between you and a destination. Those "request timed out" messages are ICMP.

Worth knowing on the security side: because ping uses ICMP, it can be weaponised — flooding a target with ICMP traffic to overwhelm it. You will see this come up when studying network attacks, and it is one of the reasons you will sometimes encounter networks that limit or block ICMP entirely.

### GRE — Generic Routing Encapsulation

GRE is a tunnelling protocol. Its job is to wrap one packet inside another so it can travel across a network that would not normally carry it.

The way I think of it: you put your packet inside an outer "envelope" that the network does accept, send it across, and the other end opens the envelope to get the original back. A common use is linking two private networks over the internet — the private traffic is wrapped up, sent across, and unwrapped at the far end.

One important point: GRE does not encrypt anything. It only wraps. That is why it is often paired with IPSec, which adds the encryption GRE lacks.

### IPSec — Internet Protocol Security

IPSec is a set of protocols that secures IP traffic by encrypting and authenticating it. It is commonly used for VPNs that connect two sites securely across the internet.

IPSec has three components, and what matters is knowing what each one does:

- **AH (Authentication Header)** — proves a packet really came from where it claims and was not tampered with. It does not encrypt, so on its own it does not hide the contents. Rarely used alone now.
- **ESP (Encapsulating Security Payload)** — does the encryption, plus authentication. Because it covers more than AH, it is the part actually used in most IPSec setups.
- **IKE (Internet Key Exchange)** — handles the setup: the two sides agree on the encryption to use and exchange keys before any data flows. It automates what would otherwise be manual key configuration.

The short version of how they fit together: IKE sets up the secure connection, and ESP protects the traffic that flows through it.

## Traffic Types

The other gap was traffic types — a way of describing how many destinations a packet is meant for. There are four.

### Unicast

One to one. A single sender, a single recipient. Most traffic is unicast — loading a web page, sending a file. Your device talks directly to one other device.

### Broadcast

One to all. A packet sent to every device on the local network at once. A device joining a network and asking "is there a DHCP server here?" uses a broadcast, because it does not yet know who to ask. Broadcasts stay on the local network — routers do not pass them on, which is part of why very large flat networks get noisy.

### Multicast

One to a group. A single stream sent only to the devices that have signed up to receive it, rather than everyone. Streaming the same video to many viewers is the classic case — one stream instead of a separate copy to each. It sits neatly between unicast and broadcast.

### Anycast

One to the nearest. The same IP address is shared by several servers in different places, and your traffic is routed to whichever one is closest or easiest to reach. This is how large public DNS services work — the same address, answered by whichever server is nearest to you. It keeps things fast without you needing to know which server you actually reached.

## Wrapping Up

ICMP is the network reporting on itself. GRE and IPSec are about tunnelling and securing traffic between places. And the four traffic types are just different answers to the same question: how many devices is this packet for?
