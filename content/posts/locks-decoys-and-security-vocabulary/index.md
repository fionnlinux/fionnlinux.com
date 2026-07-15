---
title: "Locks, Decoys, and the Words Security People Actually Use"
date: 2026-07-14T00:00:00+01:00
draft: false
description: "Physical security, honeypots and honeynets, and the CIA triad and risk vocabulary that underpins how security professionals actually talk about problems — grounded in badge access on a manufacturing floor."
tags: ["networking", "security", "comptia"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

The manufacturing floor I work on has areas a standard badge will not open. Certain doors need a specific level of access clocked onto the card, and without it, the door simply does not unlock, no matter how many times you tap it. I have walked past those doors more times than I can count without ever thinking about it as "security" in the same category as anything I study for Network+ — it was just how the building worked. It turns out it is exactly the same category, just applied to a door instead of a login screen.

## Physical Security

Physical security is the layer that exists before any network or login is even reached — controlling who can physically get to a device, a room, or a whole site in the first place. Two of the most common tools are straightforward, but worth naming properly:

**Locks** are the most basic form — badge readers, keypads, or traditional keys, all doing the same job of deciding who can physically open a door. The restricted areas at work are exactly this: a badge is a form of lock, just one that can be selectively granted or revoked per person rather than requiring a physical key to be cut and handed out.

**Cameras** do not stop anyone getting in, but they record who did, which matters for a different reason — deterrence, and evidence after the fact if something does go wrong. A locked door and a camera solve two different problems: one prevents access, the other creates accountability for whoever had it.

## Deception Technologies

This is the part of network security that genuinely surprised me, because it is not about preventing an attack at all — it is about deliberately inviting one, on your own terms.

A **honeypot** is a fake system, deliberately built to look like a real, valuable target — a server that appears to hold something worth stealing, but actually holds nothing of value and exists purely to be attacked. Anyone interacting with it is, by definition, doing something they should not be, since no legitimate user has any reason to be there at all. That makes a honeypot a way of detecting an intruder with near-total certainty, and studying exactly how they operate once they think they have gotten somewhere real.

A **honeynet** is the same idea scaled up — not one fake server, but a whole fake network of them, built to look like a genuine environment worth exploring in depth, giving a much fuller picture of how an attacker moves once they believe they are inside something real.

## The Vocabulary Behind Every Security Conversation

Before any of this can be discussed properly, a handful of words need to mean something specific, because they get used loosely in everyday conversation in a way that does not hold up in a security context.

- **Vulnerability** — a weakness that exists, whether or not anyone has found or used it yet. A door with a broken lock is a vulnerability the moment the lock breaks, regardless of whether anyone has tried it.
- **Threat** — something or someone with the potential to actually exploit a vulnerability. The broken lock is the vulnerability; a person willing to walk through it is the threat.
- **Exploit** — the specific method used to actually take advantage of a vulnerability. Not just knowing the lock is broken, but the particular way someone gets through it.
- **Risk** — the likelihood of a threat actually exploiting a vulnerability, combined with how bad the consequences would be if it happened. A vulnerability in a system nobody could ever access is a low risk; the same vulnerability on something public-facing is a much higher one.

## The CIA Triad

These three words underpin almost every security decision, and stand for something quite different from what the initials suggest:

**Confidentiality** — making sure information is only accessible to the people who are actually supposed to see it. Encryption, covered in an earlier post, is one of the main tools for enforcing this.

**Integrity** — making sure information has not been altered, whether by accident or on purpose, without that change being detected. A file that has been tampered with in transit, undetected, is an integrity failure even if nobody unauthorised ever actually read it. This is somewhere I want to go further than just the definition — RHEL documents a tool called AIDE (Advanced Intrusion Detection Environment) specifically for this, which builds a database of checksums and attributes for the files on a system, then compares the current state against that database to flag anything added, removed, or changed. I have not set it up yet, but it is exactly integrity as a concept turned into something you can actually run and check.

**Availability** — making sure a system or its data is actually accessible when it is genuinely needed. A perfectly secure system that nobody can reach when required has failed on availability, even if confidentiality and integrity are both intact.

Nearly every security control is really aimed at protecting one or more of these three things, and naming which one is actually at stake in a given situation is what makes "something is insecure" specific enough to actually act on.
