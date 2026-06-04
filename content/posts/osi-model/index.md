---
title: "Seven Layers, One Web Page, and a Movie Night That Would Not Start"
date: 2026-06-03T00:00:00+01:00
draft: false
description: "The OSI model is a wall of abstract layers until you anchor each one to something real. Here is how it clicked for me, explained through loading a web page and a movie night that would not start."
tags: ["networking", "security", "osi-model", "comptia", "soc", "troubleshooting"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

When I first met the OSI model in my CompTIA studies, it was overwhelming. Seven layers, each with a name, stacked on top of each other like Lego blocks — except nobody had told me what each block did or why it sat where it did. I could see the structure but not the point. It felt like memorising the floors of a building without knowing what happened on any of them.

What every tutorial offered was a mnemonic. "Please Do Not Throw Sausage Pizza Away" and a dozen variations, each one a trick for remembering the order of the layers. None of them helped me, because they gave me the names without the meaning. I could recite the layers and still have no idea what any of them actually did. That is the gap this post is about. Not how to list the seven layers, but how to understand them — which, for me, only happened when I stopped memorising and started attaching each layer to something real.

## What the OSI Model Actually Is

The Open Systems Interconnection model is a conceptual framework that breaks network communication into seven layers, each responsible for one part of the job of getting data from one device to another. It is not a piece of software or a physical thing. It is a way of thinking about networks — a shared map that lets people reason about where something happens, and where it might be going wrong.

It is worth saying early, because it confused me at first: the OSI model is not literally how every network is built. The internet actually runs on the TCP/IP model, which I covered in an earlier post, and that has four layers rather than seven. The OSI model is the more detailed teaching and troubleshooting framework. The two map onto each other, and CompTIA expects you to know both. Think of OSI as the diagram you reason with, and TCP/IP as the thing that is actually running.

The seven layers, from the bottom up:

1. Physical
2. Data link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

The bottom layers deal with the physical and the local — cables, signals, and getting data across a single link. The middle layers deal with addressing and reliable delivery across networks. The top layers deal with what the application and the user actually see. Data travels down the stack on the way out and back up the stack on the way in.

That description still would not have helped me much when I started. What helped was watching the layers do their jobs in something I do every day.

## How Data Actually Travels Through the Layers

Before walking through the layers one by one, there is a point that confused me at first and is worth getting straight, because once it clicks the whole model makes sense.

The seven layers are not places out in the network. They exist inside each device. Your computer has all seven layers. The server you are talking to has all seven layers. The request does not travel through one single stack — it travels *down* the stack on your device and *up* the stack on the server.

When you request a web page, the request starts at Layer 7 on your machine and moves down to Layer 1. At each layer on the way down, that layer wraps the data with its own information — the transport layer adds port details, the network layer adds IP addresses, the data link layer adds MAC addresses, and so on. This wrapping is called encapsulation. By the time it reaches Layer 1, it is a stream of bits ready to leave your device, pushed onto the cable or through the air by your network card.

It then crosses the physical network. The devices in between — switches and routers — do not climb the full stack. A switch only needs Layer 2 to read MAC addresses and forward the data on the local network. A router only climbs to Layer 3 to read IP addresses and decide where to send it next. They go up only as far as they need to do their job, then send the data back down and on its way. They never touch the upper layers, because they are directing traffic, not reading your web request.

When the bits finally arrive at the server, they enter the server's stack at Layer 1 and travel *up* to Layer 7. At each layer going up, that layer unwraps the information its counterpart added on your device — this is de-encapsulation. By the time it reaches Layer 7 on the server, the web server has the original request and can understand it. Then it sends the page back, and the whole journey happens in reverse: down the server's stack, across the network, and up yours, until the page appears in your browser.

The way I hold it in my head is packing a parcel. Going down your stack, the data is wrapped in progressively more packaging, each layer adding its own label or information — the destination address, the sender, the handling instructions. The delivery drivers and depots in between only read what they need from the outside of the parcel to know where to send it next. They never open it. At the destination, each layer of packaging is removed in turn going up the stack, until the original contents are in the hands of the person they were meant for. Down the stack is packing; up the stack is unpacking; the devices in the middle only read the outer layer they need.

## The Seven Layers, Through Loading a Web Page

Here is the model the way I now picture it — by following that web request layer by layer, from the top of my stack down to the physical medium.

**Layer 7 — Application.** This is the layer closest to you, the one you actually interact with. When your browser makes an HTTP or HTTPS request for a web page, that happens here. It is not the application itself, but the protocols the application uses to communicate — HTTP, HTTPS, DNS, and the others from the ports I wrote about previously. This is where the request begins.

**Layer 6 — Presentation.** This layer deals with how data is formatted, encrypted, and presented so both ends understand it. When your HTTPS connection encrypts the traffic, that translation between readable data and encrypted data sits here. It is the layer that makes sure data is in a form the application layer can use.

**Layer 5 — Session.** This layer manages the conversation between the two devices — opening it, keeping it going, and closing it cleanly. It is the one I found vaguest at first, and honestly it is the one most people struggle to point at. The simplest way to hold it is that it is responsible for establishing and maintaining the session that the higher layers talk over.

**Layer 4 — Transport.** This is where TCP and UDP live, the subject of an earlier post. TCP establishes a reliable, ordered connection with its three-way handshake. UDP fires data off without guarantees. The transport layer decides how the data gets carried — reliably or quickly — and breaks it into manageable segments.

**Layer 3 — Network.** This is the layer of IP addresses and routing. It is responsible for getting data across different networks, choosing a path from your device to the destination, however many networks sit in between. The device that works at this layer is the router. If Layer 2 moves data within a local network, Layer 3 moves it between networks.

**Layer 2 — Data link.** This layer handles communication between devices on the same local network, using MAC addresses rather than IP addresses. The device that works here is the switch, directing traffic between machines on the local network. It takes the data from the network layer and frames it for the physical hop to the next device.

**Layer 1 — Physical.** The bottom layer. The actual cables, the electrical or optical signals, the WiFi radio waves — the raw transmission of bits across a physical medium. This is your network card pushing the data out as signals: a wired card sending electrical signals down an Ethernet cable, or a wireless card sending radio waves to the access point. Nothing clever, just the physical movement of ones and zeros.

## Where It Stopped Being Theory — A Movie Night That Would Not Start

The OSI model earns its keep when something breaks. This is where I actually used it, even if I did not announce to myself at the time that I was "using the OSI model."

It was our weekly movie night. For a change, the kids wanted to watch in their room on their own computer rather than the usual screen. My wife went to load Amazon Prime on their machine and the page would not come up — it threw an error instead of playing anything. The natural assumption in that moment is that the internet is down.

But I knew it was not, and here is where the layered thinking kicked in without me forcing it. My phone was working fine on the same network, so the connection to the internet was up. That ruled out the bottom of the stack straight away — the physical connection, the local network, the routing out to the internet were all fine, because another device was happily online. Then I tried loading the same Amazon Prime page on my phone, and it worked. So it was not Amazon being down, and it was not a general connectivity problem. The fault was specific to the kids' device.

That is when it clicked. The kids' devices all use a NextDNS profile I set up, with content filtering that includes a block list — and Amazon Prime was on it. Their machine was asking for the address of the Amazon Prime domain, and NextDNS was refusing to resolve it, so the page could not load. Every lower layer was working perfectly. The problem was right at the top of the stack, at the application layer, where DNS does its name resolution — and it was not even a fault, it was a filter I had configured myself and forgotten about.

What I had done, without naming it, was divide and conquer. Instead of guessing randomly, I worked through the layers by elimination. Is the physical connection up? Yes, my phone is online. Is it the network or the service? No, the page loads on my phone. Is it specific to this device? Yes. What is different about this device? Its DNS. Each test ruled out a layer until only one explanation was left.

That is the real value of the OSI model. It is not just something to memorise for an exam. It is a structured way to ask "where in this stack is the problem?" rather than panicking and rebooting everything in sight.

## Using the Model to Troubleshoot

The technique I stumbled into has a name in networking — and CompTIA expects you to know it. Divide and conquer means starting in the middle of the stack and working up or down based on what you find, rather than testing every layer in order. Other approaches work from the bottom up (start at the physical layer and climb) or the top down (start at the application and descend).

The power of it is that each layer suggests its own checks. In practice the top three layers are often dealt with together, since session and presentation problems usually surface as application-layer symptoms — so the checks below focus on the layers you can most cleanly isolate:

- **Layer 1 problems** — is the cable plugged in, is the WiFi connected, is there a link light? Physical things.
- **Layer 2 problems** — is the switch working, is the device getting a connection on the local network?
- **Layer 3 problems** — does the device have a valid IP address, can it reach other networks, is routing working? A `ping` lives around here.
- **Layer 4 problems** — is the right port open, is the connection being established or refused?
- **Layer 7 problems** — is it the application, the service, or as in my case, DNS name resolution failing or being blocked?

Knowing which layer a symptom points to turns troubleshooting from guesswork into a process. A cable issue and a DNS issue produce different symptoms, and the model helps you tell them apart.

## A Note on Security

The layered view is useful for security too, and this is where it connects to the direction I am heading. Different threats target different layers, and different defences sit at different layers. A few examples to make the idea concrete:

- **Layer 1 and 2** — physical access to a network port, or attacks on the local network like MAC spoofing.
- **Layer 3** — attacks involving IP, such as certain kinds of spoofing or routing manipulation.
- **Layer 4** — the SYN flood I mentioned in my TCP/IP post is a transport-layer attack, abusing the handshake.
- **Layer 7** — many of the attacks a SOC analyst sees day to day live up here, where the applications are, such as attacks against web services.

Understanding where a given control operates, and where a given attack is aimed, is a genuinely useful lens for security work, not just a way to pass a question on an exam. A firewall filtering by port and IP is largely working at the lower and middle layers. A web application firewall inspecting HTTP requests is working at the application layer. The model tells you where each one sits.

## Where It Landed

The OSI model went from the most abstract, frustrating thing in my early studies to something I actually reach for when something breaks. That shift came entirely from anchoring each layer to something real — a router, a switch, a cable, a web request, a movie night that would not start. The model is only useful if you understand it, and you only understand it once you have seen it do something.
