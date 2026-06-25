---
title: "Disaster Recovery — Planning for When Things Go Wrong"
date: 2026-06-25T00:00:00+01:00
draft: false
description: "RPO, RTO, MTTR and MTBF, recovery sites, high-availability approaches, and the testing that proves a disaster recovery plan actually works."
tags: ["networking", "comptia"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
showComments: true
---

Networks fail. Hardware breaks, power is lost, sites flood, and sometimes an entire location goes offline. Disaster recovery is the planning that decides what happens next — how quickly things come back, how much is lost in the process, and what infrastructure is ready to take over. This post covers the concepts and measurements that make that planning concrete rather than wishful.

---

## DR metrics

Disaster recovery planning relies on a handful of measurements. They sound similar at first, so it's worth pinning down exactly what sets each one apart — the exam tests whether you can tell them apart.

**RPO (Recovery Point Objective)** is about data loss. It answers the question: how much data can we afford to lose, measured in time? If your RPO is one hour, you need backups (or replication) happening at least every hour, because in a disaster you could lose up to an hour's worth of data. A shorter RPO means more frequent backups and less tolerance for losing data.

**RTO (Recovery Time Objective)** is about downtime. It answers a different question: how long can we afford to be down before we're recovered and running again? If your RTO is four hours, the recovery process — restoring systems, switching to backup infrastructure, getting back online — has to be completed within four hours. A shorter RTO usually means investing in faster recovery infrastructure.

The simplest way to keep these two apart: **RPO looks backwards** (how far back to the last good data point), while **RTO looks forwards** (how long until we're running again).

**MTTR (Mean Time To Repair)** is the average time it takes to repair a failed component or system and return it to service. It's a measure of how quickly things get fixed once they break. A lower MTTR means faster repairs and less downtime per failure.

**MTBF (Mean Time Between Failures)** is the average time a component runs before it fails. It's a measure of reliability — how long, on average, something works before breaking. A higher MTBF means a more reliable component. These two are often paired: MTBF tells you how often failures happen, MTTR tells you how long they take to fix.

---

## DR sites

If a primary site goes down entirely, a disaster recovery site is where operations move to. These come in three types, and the difference between them is a trade-off between cost and how quickly they can take over.

**Cold site** is the cheapest and the slowest. It's essentially a physical location with the basics — space, power, cooling, connectivity — but no equipment set up and ready to go. In a disaster, you'd need to bring in hardware, install and configure everything, and restore data before it's operational. Cheap to maintain, but recovery takes a long time.

**Warm site** sits in the middle. It has hardware already in place and some configuration done, but it isn't fully up to date or running. Getting it operational means bringing systems current and restoring recent data — faster than a cold site, but not instant. A reasonable balance of cost and recovery speed.

**Hot site** is the most expensive and the fastest. It's a fully equipped, fully configured duplicate of the primary site, kept current with live or near-live data, ready to take over almost immediately. Expensive to run because you're essentially maintaining a second working environment, but downtime is minimal when disaster strikes.

The choice between them comes back to the RTO: a tight recovery time objective pushes you toward a hot site, while a more relaxed one might be satisfied by a warm or cold site at lower cost.

---

## High-availability approaches

High availability is about avoiding downtime in the first place, by running redundant systems so that if one fails, another carries on. There are two main approaches.

**Active-active** has multiple systems all running and handling work at the same time. The load is shared between them, and if one fails, the others absorb its share. This makes good use of all the infrastructure — nothing sits idle — and handles failure smoothly, though it's more complex to set up and keep balanced.

**Active-passive** has one system handling the work while a second stands by, ready to take over if the first fails. The passive system isn't doing anything useful day to day — it's waiting in reserve. Simpler to set up than active-active, but the standby capacity sits idle until it's needed.

The trade-off is straightforward: active-active uses resources more efficiently and shares load, while active-passive is simpler but keeps capacity in reserve doing nothing until a failure occurs.

---

## Testing

A disaster recovery plan that's never been tested is really just a hopeful document. Testing is what proves the plan works before a real disaster forces the issue.

**Tabletop exercises** are discussion-based. The people involved sit down and walk through a disaster scenario together, talking through what they'd each do, step by step. No systems are actually touched — it's about checking that the plan makes sense, that everyone understands their role, and that there are no obvious gaps. Low-risk and low-cost, and good for finding holes in the plan itself.

**Validation tests** go further and actually test that the recovery mechanisms work — confirming that backups restore correctly, that failover to a DR site functions, that the systems genuinely come back as the plan says they will. This is where the proof is in the pudding: a plan can look watertight on paper and still fall apart the moment it's put to use, and a validation test is what surfaces that before a real disaster does.

---

## Matching the plan to what's at stake

How much a disaster recovery plan is worth investing in depends entirely on what's being protected. The metrics give you concrete targets to design around — how much data you can lose, how long you can be down, how reliable your kit is — but the right targets vary enormously from one organisation to the next.

At one end, think about critical national infrastructure — the NHS, government services, banking, emergency services. For systems like these, downtime isn't an inconvenience, it can be a genuine risk to people's safety or to essential services that millions rely on. That justifies the expense of hot sites, active-active redundancy, and near-zero RPO and RTO targets. The cost of all that resilience is significant, but the cost of being down is far greater.

At the other end, a small business with a website that's mostly a brochure can tolerate being offline for a day without lasting harm. A cold site and a nightly backup might be perfectly adequate, and spending heavily on hot-site redundancy would be money wasted on a risk that doesn't really exist for them.

The skill in disaster recovery isn't applying the most robust option everywhere — it's matching the level of protection to the actual stakes. The metrics, the sites, and the high-availability approaches are the tools; deciding how much of each is warranted is the real judgement, and it's a judgement that comes up constantly in cloud and infrastructure work.
