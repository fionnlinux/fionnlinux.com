---
title: "Organisational Processes — How Networks Get Documented and Managed"
date: 2026-06-22T00:00:00+01:00
draft: false
description: "Documentation, life-cycle management, change management, and configuration management — the processes that keep a network running beyond the initial setup."
tags: ["networking", "comptia"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
showComments: true
---

Getting a network operational is just the beginning. What follows is the ongoing work of running it reliably — keeping track of what's in it, managing changes as they happen, and planning for when devices and software get old. This objective is about the processes and documentation that make that possible.

---

## Documentation

Good documentation means that anyone coming to a network fresh can understand it without having to reverse-engineer everything from scratch. It also means that when something breaks at 2am, the person fixing it isn't working from memory. It's something I'm actively trying to get better at — writing this site is part of that, learning to explain things clearly and leave a trail that's actually useful rather than just a record that something happened.

**Physical vs. logical diagrams** capture two different views of the same network. A physical diagram shows the actual hardware — which devices are physically connected to which, where cables run, what sits in which rack. A logical diagram shows how the network is addressed and structured — IP addresses, VLANs, routing boundaries, subnet layouts — without necessarily reflecting the physical layout. Both matter, and they can look very different from each other. Two servers might sit side by side in the same rack on the physical diagram, but on the logical diagram they're in completely separate subnets with a firewall between them. The physical diagram tells you where to go; the logical diagram tells you how everything relates and what's allowed to talk to what.

**Rack diagrams** document what's installed in each rack, in what order, and how much space is used. They're used for planning — knowing whether there's space for new equipment before ordering it — and for reference during maintenance.

**Cable maps and diagrams** record where cables run, what connects to what, and which ports are in use. Without this, tracing a fault to a specific cable or port becomes a process of elimination through physical inspection. It's similar in principle to wiring diagrams in electrical work — when I was doing electrical installation, following a wiring diagram to connect up a panel with dozens of small connections was the only way to do it accurately. The documentation is what makes the job possible, and what makes it possible for someone else to pick up where you left off.

**Network diagrams** can be drawn at different layers:
- **Layer 1** shows the physical connections — which devices are cabled together
- **Layer 2** shows the switching topology — VLANs, trunks, spanning tree root
- **Layer 3** shows the routing topology — subnets, IP addressing, routing protocols, gateways

Having diagrams at each layer means you can quickly narrow down where a problem sits — is it a physical connection issue, a switching issue, or a routing issue?

**Asset inventory** is a record of everything in the network — not just that it exists, but the details that matter for managing it:
- **Hardware** — make, model, serial numbers, physical location
- **Software** — what's installed, what version
- **Licensing** — what licences are held, when they expire, what they cover
- **Warranty support** — what's covered under warranty and until when

Without an accurate asset inventory, it's difficult to know what needs updating, what's coming out of warranty, or what you actually own.

**IP address management (IPAM)** is the process of tracking which IP addresses are assigned, to what, and what's still available. In a small network this might just be a spreadsheet. In a large one it's usually a dedicated tool. Either way, without it address conflicts become common and troubleshooting gets harder.

**Service-level agreement (SLA)** is a formal agreement between a service provider and a customer defining what level of service is expected — uptime guarantees, response times to faults, and what happens if those targets aren't met. A real example I deal with directly: the WordPress hosting for a client site I manage runs on Rocket.net, which offers a 99.99% uptime guarantee as part of their SLA. That's a meaningful commitment — 99.99% uptime translates to less than an hour of downtime per year. From a networking perspective, SLAs define the standards the infrastructure is expected to meet and what the consequences are if it doesn't.

**Wireless survey/heat map** is a record of wireless signal coverage across a physical space — where the signal is strong, where it's weak, and where dead spots exist. Used when planning access point placement and when diagnosing coverage complaints. I'd assume something like this exists at my workplace — there have been ongoing issues with radio connectivity dropping out in certain parts of the warehouse and plant floor, exactly the kind of problem a proper wireless survey would identify and help resolve.

---

## Life-cycle management

Every piece of hardware and software has a lifespan, and managing that lifespan deliberately is part of running a network responsibly.

**End-of-life (EOL)** is the point at which a manufacturer stops actively selling or producing a product. It doesn't necessarily mean the product stops working, and support often continues for a defined period afterwards, but it signals that the product is being phased out and planning for its replacement should begin.

**End-of-support (EOS)** is the more significant point — when a manufacturer stops providing security patches, bug fixes, or technical support. Running equipment past its end-of-support date means known vulnerabilities may go unpatched indefinitely, which is a real security concern.

**Software management** covers the ongoing work of keeping software current:
- **Patches and bug fixes** address specific known issues and vulnerabilities
- **Operating system (OS)** updates keep the underlying system current and supported
- **Firmware** updates apply to the software embedded in hardware devices — switches, routers, access points — which also receive security fixes and improvements

**Decommissioning** is the process of retiring equipment from service properly. This isn't just unplugging something — it includes removing it from the network safely, updating documentation to reflect its removal, recovering any licences, and ensuring any data stored on the device is wiped before disposal.

---

## Change management

Networks change constantly — new devices, configuration updates, software patches, hardware replacements. Change management is the process that controls how those changes happen, so that they don't introduce new problems or catch people off guard.

**Request process tracking / service request** — changes to a network typically go through a formal request process. Someone raises a request describing what they want to change and why, the request is reviewed and approved before anything happens, and the change is scheduled for an appropriate time. This creates a record of what changed, when, and who authorised it. When something breaks after a change, that record is what lets you trace it back quickly.

The alternative — people making changes informally without a record — means that when something goes wrong, nobody knows what changed, when it changed, or whether the two things are connected.

---

## Configuration management

Knowing what a device is configured to do right now, and being able to restore it if something goes wrong, is what configuration management handles.

**Production configuration** is the current live configuration of a device — the settings it's actually running with in the network. This needs to be documented and kept current, because if a device fails and needs to be replaced, the replacement needs to know what configuration to run.

**Backup configuration** is a saved copy of the configuration, taken regularly, so that if a device is misconfigured or fails it can be restored to a known good state. Without backups, recovering from a configuration error means rebuilding from scratch.

**Baseline/golden configuration** is a standard reference configuration — a known good starting point that represents how a device of that type should be set up. New devices can be built from the baseline, and existing devices can be checked against it to detect drift (settings that have changed from the standard, whether intentionally or not).

---

## Why this matters

The processes in this objective exist because networks aren't static. Devices get old, configurations drift, people make changes, and hardware fails. Documentation, life-cycle management, change management, and configuration management are the structures that keep all of that manageable — and that turn a network from something one person holds in their head into something an organisation can actually maintain. Done properly, someone with no prior knowledge of the network can come in, find what they need, and keep things running — even if everyone who originally built it has long since moved on.
