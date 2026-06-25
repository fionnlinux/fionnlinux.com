---
title: "Network Monitoring — Knowing What Your Network Is Actually Doing"
date: 2026-06-24T00:00:00+01:00
draft: false
description: "SNMP, flow data, packet capture, log aggregation and SIEM, plus the monitoring solutions that turn raw data into something you can act on."
tags: ["networking", "comptia", "security"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
showComments: true
---

A network you can't see into is a network you can't really manage. Monitoring is how you find out what's happening — what's connected, how much traffic is flowing, whether something's failing, and whether anything looks wrong. This objective splits into two halves: the methods used to collect monitoring data, and the solutions that turn that data into something you can act on.

---

## Methods

### SNMP

**SNMP (Simple Network Management Protocol)** is the standard way devices report their status to a central monitoring system. Switches, routers, servers, printers — almost any network device — can be queried over SNMP to report things like interface status, traffic levels, CPU usage, and errors. A central system (often called an SNMP manager) collects this from all the devices by regularly polling them — sending a request to each device on a schedule asking for its current status — and gives you one place to see the health of the whole network.

**MIB (Management Information Base)** is the structured list of everything a device can report over SNMP. Each piece of information — interface status, uptime, error counts — has a defined identifier in the MIB. When the manager asks a device for a value, it's referring to an entry in that device's MIB.

**Traps** are the exception to the usual polling pattern. Normally the manager polls devices at intervals asking for their current status. A trap reverses that — the device sends an alert to the manager on its own when something happens, without waiting to be asked. A trap fires when an interface goes down, for example, meaning urgent events are reported immediately rather than waiting for the next poll.

**Community strings** are how older versions of SNMP handle access. A community string is essentially a password that a device and manager share — if they match, access is granted. The problem is that in SNMP v2c, community strings are sent in plain text, which means anyone capturing the traffic can read them. Many devices ship with default community strings ("public" for read access, "private" for write), and leaving those unchanged is a genuine security hole.

**Versions** matter here, and the distinction is exam-relevant:
- **v2c** is widely used but has no real security — community strings are sent in plain text and there is no encryption
- **v3** adds proper security: authentication to confirm who is making the request, and encryption so the data cannot be read in transit

The takeaway is that v3 should be used wherever security matters, and the reason is specifically that v2c offers no protection for its credentials or its data.

### Flow data

**Flow data** records information about the conversations happening on a network — which devices are talking to which, over what protocols, and how much data is moving — without capturing the actual contents of that traffic. Think of it as the network's equivalent of an itemised phone bill: it shows who connected to whom, when, and how much, but not what was said. This makes it well suited to spotting patterns and unusual behaviour — a device suddenly sending large amounts of data to an unfamiliar destination stands out in flow data.

### Packet capture

**Packet capture** goes a level deeper than flow data — it records the actual packets crossing the network, contents included. A tool like Wireshark captures this and lets you examine traffic in detail, right down to individual protocol exchanges. The file produced is often called a PCAP (from "packet capture").

This level of detail is what makes packet capture valuable for serious troubleshooting and security analysis. When you need to know exactly what happened — not just that two devices communicated, but precisely what was sent — a packet capture is what shows you. For security work, a PCAP can show the actual content of suspicious traffic, which is what you need to understand an attack rather than just detect that one occurred. The trade-off is volume: full packet capture generates a lot of data quickly, so it is usually targeted rather than left running across everything indefinitely.

### Baseline metrics

A **baseline** is a record of what normal looks like for a network — typical traffic levels, normal CPU and memory usage, usual numbers of connections at a given time of day. The value of a baseline is that it gives you something to compare against.

**Anomaly alerting/notification** builds on that baseline. Once you know what normal looks like, you can configure alerts to fire when something deviates from it — traffic spiking far above usual levels, or a device behaving in a way it normally does not. Without a baseline, there is no way to know whether what you are seeing is unusual or simply business as usual.

### Log aggregation

Individual devices each produce their own logs, but logs scattered across dozens of devices are difficult to make sense of. **Log aggregation** is the practice of collecting logs from across the network into one central place, so they can be searched, compared, and correlated together.

**Syslog** is the standard protocol for this. Devices send their log messages to a central **syslog collector**, which gathers everything in one location. This alone makes troubleshooting far easier — instead of logging into each device individually, you have one searchable record of what happened across the whole network.

**SIEM (Security Information and Event Management)** takes log aggregation further and is where this becomes a security tool. A SIEM collects logs from across the environment and actively analyses them — correlating events from different sources, looking for patterns that indicate a security problem, and raising alerts. The importance is in the correlation: a single failed login on one server means little, but the same account failing across twenty servers in two minutes is a pattern a SIEM can catch and flag, where looking at any one log in isolation would miss it entirely.

### API integration

**API (Application Programming Interface) integration** allows monitoring systems to connect to other tools and platforms programmatically. A good security example is a SIEM pulling in live threat intelligence feeds over an API — lists of known malicious IP addresses, domains, and file hashes from external sources. The SIEM can then cross-reference activity in your own logs against that live data automatically, asking not just "is this unusual for us?" but "is this address known to be malicious right now?" — and it stays current without anyone updating it by hand.

---

## Solutions

**Network discovery** is the process of finding out what is actually on a network — identifying devices, how they are connected, and what they are. nmap is probably the most well-known tool for this, able to scan a network and return a list of live hosts, open ports, and running services. Discovery can be:
- **Ad hoc** — run on demand, when you want a snapshot of what is currently there
- **Scheduled** — run automatically at set intervals, to keep an ongoing picture as devices come and go

**Traffic analysis** examines the flow of traffic across the network to understand what is normal, identify what is consuming bandwidth, and spot unusual patterns.

**Performance monitoring** tracks how well the network and its devices are performing — latency, throughput, error rates, resource usage — so that degradation can be caught and addressed before it becomes a visible problem.

**Availability monitoring** is concerned with whether things are simply up or down. It continuously checks that devices and services are reachable and responding, and alerts when something goes offline. This is the monitoring that underpins uptime guarantees. Uptime Kuma is a good open source example — a self-hosted tool that regularly checks the services you point it at and notifies you the moment one stops responding.

**Configuration monitoring** watches for changes to device configurations, flagging when something has been altered — it is how configuration drift gets detected, and how an unauthorised or accidental change gets noticed rather than sitting unnoticed until it causes a problem. It is worth distinguishing from FIM (File Integrity Monitoring), which watches for changes to files on a system and is more of a security/endpoint concept. A common approach for network device configs is to store them in Git, so that every change is version-controlled and any difference from the last known good state can be seen at a glance and rolled back if needed.

---

## Why this matters

Monitoring is what turns a network from something you hope is working into something you can actually see. The methods covered here — SNMP, flow data, packet capture, log aggregation — are how the raw information gets collected. The solutions are what that information is used for: finding what is on the network, catching problems before users do, keeping track of availability, and spotting when something has changed that should not have. Getting comfortable with these concepts early is worth the time — cloud and security work both depend on visibility, and a solid grasp of monitoring is part of the foundation they are built on.
