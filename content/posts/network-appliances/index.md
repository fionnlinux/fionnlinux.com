---
title: "From Router to Reverse Proxy — Understanding the Devices Behind a Network"
date: 2026-06-04T00:00:00+01:00
draft: false
description: "Routers, switches, firewalls, IDS/IPS, proxies, load balancers and more. A tour of the appliances that make a network work, grounded in the ones I already run at home."
tags: ["networking", "security", "appliances", "comptia", "soc", "homelab"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

A network is not one thing. It is a collection of devices, each doing a specific job, working together to move data around and keep it safe. The Network+ objectives call these networking appliances, and when I first saw the list it was a wall of names with no clear shape — router, switch, firewall, IDS, IPS, load balancer, proxy, and on it goes.

What changed it, the same as every other topic I have written about, was realising how many of them I already use. Some I have configured directly. Some are running quietly behind things I set up months ago without thinking of them as "appliances" at all. This post is a tour of the main ones, grounded where I can in the versions I actually run, because that is what turned the list from vocabulary into something I understand.

A quick note before the tour: these used to be physical boxes you bought and racked. Increasingly they are virtual — software versions running on general-purpose hardware or in the cloud. A firewall can be a dedicated appliance or a piece of software. The function is what matters, not whether it is a physical box.

## The Ones That Move Traffic

**Router.** A router connects different networks together and directs traffic between them. It works at Layer 3 of the OSI model, dealing in IP addresses, choosing the path data takes from one network to another. The router in your home connects your local network to your ISP, and through them to the rest of the internet.

I have a particular interest in routers because of a project I am planning. Most people, myself included until recently, use the all-in-one box the ISP provides — a router, switch, firewall, and wireless access point combined into one unit. My goal is to replace that with a dedicated router and firewall built on OPNsense running on a Protectli device. The reason is control. A dedicated firewall lets me segment my network properly with VLANs, run my own VPN, and apply far more granular rules than a consumer box allows. Researching that build is a lot of what made these appliances real to me rather than abstract — once you plan to run your own, you have to understand what each part actually does.

**Switch.** A switch connects devices within the same local network and directs traffic between them using MAC addresses rather than IP addresses. It works at Layer 2. Where a router moves data between networks, a switch moves it between devices on the same network. A managed switch can be configured with features like VLANs and port security; an unmanaged one simply forwards traffic with no configuration.

## The Ones That Protect

**Firewall.** A firewall controls what traffic is allowed in and out of a network based on a set of rules. Depending on type it can work at several layers — a basic packet filter looks at IP addresses and ports at Layers 3 and 4, while a next-generation firewall can inspect traffic right up to Layer 7. It is the gatekeeper deciding what gets through.

I have already met firewalls in a small but real way. My kids sometimes host game servers for each other on our local network, and to let their devices connect I have had to open specific ports on each machine's firewall. That is firewall rule management in miniature — deciding deliberately what traffic to permit and to whom. The OPNsense build will be the same idea at a much larger scale, controlling traffic for the whole network from one place.

**IDS — Intrusion Detection System.** An IDS monitors network traffic and raises an alert when it sees something suspicious. The key word is detection. It watches and reports, but it does not stop anything. Think of it as a security camera — it records the break-in and sounds the alarm, but it does not lock the door.

**IPS — Intrusion Prevention System.** An IPS does what an IDS does, but it sits inline in the flow of traffic and can actively block what it judges to be malicious before it reaches its target. If an IDS is a security camera, an IPS is a security guard who can physically stop someone getting through. The trade-off is that because it sits in the traffic path, a poorly configured IPS can block legitimate traffic and cause problems of its own.

The distinction between these two is one I expect to learn properly through doing rather than reading. A big part of my homelab plan is setting up a Wazuh SIEM, which includes intrusion detection capability, and using it to actually see this in action. I have not done it yet — but understanding the difference now means that when I do, I will know what I am looking at. Detection raises an alert for a human to investigate; prevention acts on its own. That difference matters enormously in a SOC, where you are constantly balancing catching threats against not breaking legitimate traffic.

**Proxy.** A proxy is an intermediary that sits between a client and a destination and makes requests on the client's behalf. Your request goes to the proxy, the proxy fetches the resource, and it passes the result back to you. The destination only ever sees the proxy, not you.

There are two kinds, and the distinction caught me out at first because they do quite different jobs under the same name. A forward proxy sits in front of clients — a workplace web filter that controls and monitors what employees can browse is a forward proxy. A reverse proxy sits in front of servers, handling incoming requests on their behalf. I first ran into the reverse proxy idea when reading about hosting a game server from home — the advice was always to put something like Nginx in front as a reverse proxy, so that people connecting from outside reach the proxy rather than your home machine directly, shielding your actual server. It turns out I am already benefiting from one: Cloudflare sits in front of my website as a reverse proxy, taking visitors' requests and passing them to where the site is actually hosted, without exposing the origin directly.

## The Ones That Scale and Speed Things Up

**Load balancer.** A load balancer distributes incoming traffic across multiple servers so that no single server is overwhelmed. As well as handling high demand, it improves availability — if one server fails, the load balancer simply routes traffic to the others. Any service that needs to stay up under heavy load or survive a server failure will sit behind one.

**CDN — Content Delivery Network.** A CDN is a network of servers spread across many locations that cache content close to users. Instead of every request travelling all the way to the origin server, content is served from a node geographically near the visitor, which means faster load times and less strain on the origin. This is not just theory for me — my site uses Cloudflare, and using them for DNS means I get their CDN included. Visitors are served cached content from a Cloudflare edge location near them rather than reaching all the way to where the site is hosted. It is part of why the site loads quickly. The same service is acting as my registrar, my CDN, and my reverse proxy at once, which is a neat illustration of how these functions overlap in practice.

## The Ones That Store

This is the pair I know least well, because I do not yet use either — so I will be honest about that rather than pretend otherwise.

**NAS — Network Attached Storage.** A NAS is a storage device connected to the network that multiple devices can access, essentially a shared drive available over the network. It appears to users as a file share. Common in homes and small businesses for centralised storage and backups. This is one I expect to add to my homelab eventually, at which point it will stop being abstract.

**SAN — Storage Area Network.** A SAN is a high-speed network dedicated entirely to storage, used in enterprise environments. Unlike a NAS, which presents itself as a file share, a SAN presents storage to a server as though it were a locally attached disk. It is fast, expensive, and used where performance and reliability are critical — data centres, hospitals, banks. The simplest way I hold the difference: a NAS is a shared folder on the network, a SAN is a dedicated storage network that servers treat as their own local disks.

## The Ones That Handle Wireless

**Wireless Access Point (AP).** An access point provides wireless connectivity to a wired network. It connects to the network by cable and broadcasts a WiFi signal for devices to join. A home router usually has one built in. In larger buildings, APs are separate devices placed throughout to give coverage.

**Wireless Controller.** When you have many access points, configuring each one individually becomes impractical. A wireless controller manages them centrally — letting you configure, monitor, and update all the APs from one place. I see this at work, where there are a large number of access points across the building; managing that many individually would be unworkable, so a controller is almost certainly handling them centrally.

## The Functions Worth Knowing

The objectives also group a few functions alongside the appliances. These are not boxes so much as capabilities.

**VPN — Virtual Private Network.** Covered in depth in an earlier post. In the appliance context, a VPN concentrator is a dedicated device that handles many VPN connections at once in an enterprise.

**QoS — Quality of Service.** QoS prioritises certain types of network traffic over others. The example that makes it obvious is the workplace: you want voice and video calls to take priority over a large file download, because a call breaking up is far more disruptive than a download taking a little longer. QoS is the mechanism that makes that prioritisation happen.

**TTL — Time to Live.** A value carried in a packet that limits how long it can travel. Every router that forwards the packet reduces the TTL by one, and when it reaches zero the packet is discarded. This stops packets from circulating forever if something goes wrong with routing. The same term also appears in DNS, where it sets how long a record can be cached — which is what you are waiting on when a DNS change takes time to take effect.

## What Helped It Stick

I found these much easier to understand once I stopped reading them as definitions and started noticing where they already turn up around me. That is the approach that works for how I learn, and I hope laying it out this way is useful to someone else working through the same objectives.

If any of it still feels abstract, the best thing I can suggest is to get hands-on with something small. Log into your home router's web portal and have a look at what is there — the firewall settings, the DHCP range handing out addresses, the wireless security options, the connected devices list. It is one of the easiest places to see several of these appliances doing their jobs in one screen, all on equipment you already own. There is a lot you can explore from a home network and a spare evening, and seeing an appliance actually do its job teaches more than any definition I could write here.
