---
title: "Routing — How Networks Decide Where Traffic Goes"
date: 2026-06-15
draft: false
description: "Static routing, dynamic routing protocols, NAT, PAT, route selection, and the mechanisms that keep traffic moving reliably."
tags: ["networking", "comptia", "routing"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Switches move traffic between devices on the same network. Routers move traffic between different networks. That distinction sounds simple, and it is — but the moment you ask how a router decides where to send traffic, things get interesting fast.

This post covers routing: static routing, dynamic routing protocols, how routers choose between competing paths, NAT, PAT, and a few concepts around redundancy and virtual interfaces.

## Static vs Dynamic Routing

A router's job is to forward traffic toward its destination. To do that, it needs to know which direction to send traffic for any given network. That knowledge lives in a **routing table** — a list of known networks and how to reach them.

There are two ways a router builds that table.

**Static routing** means a network administrator manually tells the router what to do. "To reach network 10.0.2.0/24, send traffic out this interface toward that next hop." The router writes that down and follows it until someone changes it. It never adapts on its own.

Static routing suits simple networks with predictable traffic — a single branch office connecting back to a head office, for example. It is also used for specific fixed paths where you want full control. The downside is exactly that fixed nature: if something changes or a link goes down, the router keeps following the old instruction until a human steps in and updates it.

**Dynamic routing** is different. Routers running a dynamic routing protocol talk to each other, share information about the networks they can reach, and build their routing tables automatically. If a link fails, they detect it, share the updated information with each other, and find an alternative path — without any human involvement.

The two are often used together: dynamic routing handling the bulk of the network so it can adapt on its own, with static routes added for specific paths where you want fixed, predictable behaviour. They complement each other rather than competing.

## Dynamic Routing Protocols

Three protocols come up when learning this topic. You do not need to configure them to understand what they are and where each one fits.

**OSPF — Open Shortest Path First**

OSPF is one of the most widely used interior routing protocols — meaning it runs within a single organisation's network rather than between organisations. It works by having each router build a complete map of the network, then calculate the shortest path to every destination using that map.

It chooses paths using a value called **cost**. Cost is worked out from the bandwidth of each link — a faster link is given a lower cost, a slower link a higher cost. When OSPF has more than one way to reach a destination, it adds up the cost along each possible path and picks the one with the lowest total. In plain terms, it prefers the fastest route, not necessarily the one with the fewest hops. A path that takes two fast links can beat a path across one slow link.

OSPF is an open standard, which means it works across equipment from any vendor rather than being tied to one manufacturer. It is the kind of protocol most people learning this are most likely to come across in their own home network or a small setup, rather than the larger systems the other protocols tend to run on.

**EIGRP — Enhanced Interior Gateway Routing Protocol**

EIGRP is Cisco proprietary — it only works between Cisco devices. It chooses paths using a metric that combines several factors, mainly bandwidth and delay, rather than bandwidth alone. It is worth knowing by name, though you will mostly meet it inside Cisco environments specifically.

**BGP — Border Gateway Protocol**

BGP is the protocol that holds the internet together, so it is worth slowing down to explain.

The internet is not one network. It is a vast collection of separate networks — every internet provider, every large company, every cloud platform runs its own. Each of these is responsible for its own patch and needs a way to tell the others which destinations it can reach. That announcement is called **advertising a route** — essentially a network saying "traffic for these addresses can come through me."

BGP is the language those separate networks use to make those announcements to each other. When you load a website hosted on the other side of the world, your traffic crosses many of these separate networks to get there, and BGP is what allowed each one to learn the path. OSPF and EIGRP work inside a single network; BGP works between them.

It comes up a lot in cloud work. A common setup is connecting a company's own private network — the internal network in their office or data centre, not reachable from the public internet — to a cloud platform like Azure or AWS over a dedicated private link. BGP is usually what runs across that link, with each side advertising its routes to the other so traffic knows how to flow between them.

## Route Selection

A router often knows more than one path to the same destination — learned from different routing protocols, from static routes, or from several dynamic sources at once. When that happens, it needs a way to choose. Three rules decide, applied in order.

**Prefix length** is checked first — how specific the matching route is. If a router knows a route to 192.168.1.0/24 and another to 192.168.1.64/28, traffic for 192.168.1.70 matches both. The /28 covers a smaller, more precise range of addresses, so it wins. It is like an address label: "London" and "14 Baker Street, London" both technically match, but the more specific one is clearly the intended destination. A router always prefers the most specific match it has.

**Administrative distance** is the tiebreaker when two routes are equally specific but come from different sources — say one static route and one learned through OSPF, both pointing at the same destination. The router needs to decide which source to believe. Administrative distance is a trust score for each source, where a lower number means more trusted. Static routes carry a very low value because an administrator set them deliberately, so they are trusted over routes a protocol worked out automatically. This is simply how a router settles the situation when two sources tell it different things about the same destination.

**Metric** is the final tiebreaker, used within a single routing protocol. If OSPF itself knows two equally specific paths to the same place, it compares the metric of each and takes the lower one. How that metric is worked out depends on the protocol — OSPF adds up the cost of every link along a path, where each link's cost comes from its bandwidth, and the path with the lowest total wins. EIGRP works it out differently, combining bandwidth and delay into a single figure. The detail varies, but the principle is the same: each protocol has its own way of scoring how good a path is, and the better score wins. Metric only comes into play once prefix length and administrative distance have not already decided things.

In plain terms: most specific match first, most trusted source second, best metric third.

## Address Translation — NAT and PAT

Most devices on a home or office network use private IP addresses — ranges set aside for internal use that cannot be used directly on the public internet. To reach the internet, those private addresses need to be translated to a public one. That is what **NAT — Network Address Translation** does.

NAT sits at the boundary between the private network and the public internet, usually on a router or firewall. Outgoing traffic has its private source address rewritten to the public IP. Incoming responses have the destination rewritten back to the private address of the device that made the request.

The clearest way I have seen this in practice is working with virtual machines in KVM. When you create a VM in KVM, it gets a private IP on a virtual network — typically in the 192.168.122.x range. When that VM needs to reach the internet, KVM's NAT translates its private address to the host machine's real IP. The VM can browse, download packages, and talk to outside services, all through the one IP the rest of the world sees. Responses come back to that IP and NAT passes them back to the right VM.

**PAT — Port Address Translation** takes this further. Rather than mapping one private IP to one public IP, PAT lets many private IPs share a single public IP, told apart by port numbers. Each outgoing connection is tracked by its port — when the response returns, PAT uses that port to know which internal device to send it back to.

This is what your home router does. Every device in the house — phone, laptop, TV, console — shares one public IP address, and PAT keeps them all straight. It is sometimes called **NAT overload**, because it is NAT stretched to handle many devices behind one address.

## First Hop Redundancy — FHRP and VIP

Devices on a network are configured with a **default gateway** — the router they hand traffic to whenever the destination is outside their local network. That works fine until the router goes down, at which point everything on that network loses its way out.

**FHRP — First Hop Redundancy Protocol** solves this. Two or more routers share a **Virtual IP (VIP)** — a single IP address that acts as the default gateway for the network. One router is active and handles the traffic; another waits in standby. If the active router fails, the standby takes over the same VIP and traffic keeps flowing. The devices on the network never notice, because the gateway address they were configured with has not changed — only the physical router answering to it has.

HSRP (Cisco) and VRRP (an open standard) are the common implementations. The exam uses FHRP as the umbrella term covering them.

## Subinterfaces

A physical network port on a router normally handles one connection. A **subinterface** splits that single physical port into several logical ones in software, so one cable can carry traffic for multiple separate networks at once.

This is most often used to route between VLANs. VLANs split one physical network into several separate logical ones, but devices on different VLANs still need a router to pass traffic between them. Rather than dedicating a separate physical port to each VLAN, you give the router one port divided into subinterfaces — one per VLAN. The switch sends the router all the VLAN traffic over a single link, the router routes between the VLANs using its subinterfaces, and sends it back. Because all of this happens over one physical connection, the setup is commonly called **router-on-a-stick**. It is a tidy way to handle inter-VLAN routing without needing extra hardware.

## Bringing It Together

Routing is the mechanism that connects networks together and keeps traffic moving even when something goes wrong. Static routing is simple and manual — you set the path and it stays put. Dynamic routing is automatic and adaptive — routers share what they know and reroute around failures on their own. NAT and PAT bridge the gap between private addresses and the public internet, something happening invisibly on nearly every network, including the virtual ones running on your own machine. And route selection, FHRP, and subinterfaces are the finer detail that makes all of it hold together reliably — choosing the best path, surviving a failed router, and splitting one port across many networks.
