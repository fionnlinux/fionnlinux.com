---
title: "Why Some Data Can't Just Live Anywhere, and Why Some Devices Get Their Own Network"
date: 2026-07-15T00:00:00+01:00
draft: false
description: "Data locality, PCI DSS, and GDPR, alongside network segmentation for IoT, industrial systems, guests, and BYOD — the rules and boundaries that exist around a network rather than inside its traffic."
tags: ["networking", "security", "comptia"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Everything covered in this section so far has been about the network itself — encrypting it, authenticating onto it, physically locking it down. This part is different. It is about the rules that exist around a network rather than inside its traffic, and about why some devices are deliberately kept apart from others even when nothing has gone wrong yet.

## Audits and Regulatory Compliance

**Data locality** is the requirement that certain data has to physically stay within a particular country or region, rather than being stored or processed wherever happens to be cheapest or most convenient. This matters more than it sounds like it should, because cloud infrastructure is genuinely global by default — a server in one country can easily end up holding data that legally is not allowed to be there, unless the setup specifically accounts for it.

**PCI DSS** (Payment Card Industry Data Security Standard) is a set of requirements that anything handling card payment data has to follow — encryption standards, access controls, and regular audits, all aimed at the same goal: reducing how often card data actually gets exposed or stolen. It is not a law, but a standard enforced by the card industry itself, and failing to comply can mean losing the ability to process card payments at all.

**GDPR** (General Data Protection Regulation) is the one I will actually be living under once relocated to the UK, even without a strong technical story to tell about it yet. It governs how personal data belonging to people in the UK and EU has to be collected, stored, and handled — including things like a person's right to know what data is held about them, and to have it deleted on request. Data locality and GDPR overlap heavily in practice, since where data physically lives is often exactly what determines which regulation applies to it in the first place.

## Network Segmentation

Segmentation is the deliberate practice of splitting a network into separate zones, so that different types of device are never sitting on the same flat network as everything else. The logic behind it is straightforward once stated plainly: if something on one segment is compromised, segmentation is what stops that compromise from spreading freely to everything else, rather than one bad device putting the whole network at risk.

A few categories come up specifically:

**IoT and IIoT** (Internet of Things and Industrial Internet of Things) devices — smart plugs, cameras, sensors — are frequently under-secured compared to a proper computer, often running outdated software with no realistic way to patch it. Segmenting them onto their own network means a compromised smart camera cannot casually reach a laptop or server sitting on the main network, because there is no direct path between the two.

**SCADA, ICS, and OT** (Supervisory Control and Data Acquisition, Industrial Control Systems, and Operational Technology) refer to the systems that actually control physical processes — machinery, manufacturing equipment, industrial infrastructure. These typically cannot be patched or rebooted casually the way an office laptop can, since doing so risks interrupting a physical process that may be expensive or dangerous to stop. Segmentation here is not just about limiting damage from a breach, but about keeping fragile, hard-to-update systems away from the kind of everyday network traffic that could disrupt them even accidentally.

**Guest** networks separate visitors from the actual internal network entirely. A guest only needs internet access, not access to anything else sitting on the network, so giving them their own segment means there is nothing internal for a guest device to accidentally or deliberately reach.

**BYOD** (Bring Your Own Device) covers personal devices — a phone or laptop someone owns themselves — being used to access work resources. These sit in an awkward middle ground: more trusted than a random guest, but not owned or fully controlled by the organisation the way an official work device is. Segmentation gives BYOD devices a defined, limited space to operate in, rather than the full access an owned and managed device might otherwise get.

## The Shared Idea Underneath Both Halves

Compliance and segmentation look like two unrelated topics, but they are really answering the same underlying question from two different directions: what is allowed to touch what, and under what conditions. Compliance answers it from the outside in — rules imposed by regulation or industry standard, regardless of how the network is actually built. Segmentation answers it from the inside out — the network's own structure deciding what can reach what, enforced by design rather than by policy on paper. A network can follow every applicable regulation and still be poorly segmented, and a well-segmented network can still fail an audit if data ends up somewhere it legally should not be. Both have to hold at once.
