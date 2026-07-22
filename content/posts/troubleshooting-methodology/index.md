---
title: "The Method Behind Fixing Anything — Walking Through the Troubleshooting Steps"
date: 2026-07-21T00:00:00+01:00
draft: false
description: "The seven-step troubleshooting methodology, walked through end to end using the same NextDNS block that showed up in an earlier post on the OSI model."
tags: ["networking", "comptia"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

An earlier post described a moment where a family member's device could not load Amazon Prime, while every other device on the network worked fine. [That post]({{< ref "posts/osi-model" >}}) walked through the layer-by-layer elimination that found the cause — DNS filtering, not a fault. This one uses the same story for something different: the formal seven-step methodology sitting around that whole process, most of which happened silently and never made it into the original post at all.

## Identify the Problem

This step is less about guessing and more about actually gathering the full picture first. What is the symptom? A specific site would not load. Who noticed it, and on which device? Question users, in this case simply asking what was happening and when. Has anything changed recently? Nothing had — no new settings, no new app installed. Could the problem be duplicated? Yes, reliably, every time that device tried to load that one site. And if there had been multiple issues reported at once, the methodology says to approach each one individually rather than assuming they share a single cause.

None of that requires technical skill yet. It is just refusing to guess at a cause before actually establishing what is happening.

## Establish a Theory of Probable Cause

With the symptom confirmed, the next step is forming an actual theory rather than jumping straight to a fix. Question the obvious first — was the site itself down? A quick check on another device ruled that out immediately.

From there, the methodology allows a few different approaches: working top-to-bottom or bottom-to-top through the OSI model, or dividing and conquering by testing in the middle and narrowing from there. That earlier post walked through exactly this part of the story in full — testing another device, narrowing the fault down layer by layer until DNS was the only explanation left. That elimination process is the theory-forming step in practice, so it is worth reading there rather than repeating here.

## Test the Theory

That narrowed theory pointed toward DNS, since the affected device was configured to use NextDNS directly. Testing it meant checking the NextDNS dashboard itself, and there it was — the domain was showing up as blocked in the logs.

Theory confirmed. If it had not been confirmed, the methodology is clear about what happens next: form a new theory, or escalate if nothing plausible is left. In this case there was no need for either.

## Establish a Plan of Action

Confirming the cause is not the same as fixing it. The plan here was small and low-risk: remove that specific domain from the NextDNS blocklist for that profile. Worth noting as part of this step, even for something this minor: considering what else the change might affect. Unblocking one domain on one profile was not going to touch anything else running through that same resolver, which is exactly the kind of check this step exists to force, even when the answer turns out to be "nothing."

## Implement the Solution

Straightforward here — the domain was removed from the block list directly in the NextDNS dashboard. No escalation needed, since this was well within what I could resolve myself.

## Verify Full System Functionality

The fix is not finished until it is actually confirmed working, not just assumed to be. The site was tried again on the affected device, and it loaded normally. The methodology also calls for considering preventive measures at this stage — in this case, actually making a note of which domains were filtered and why, so a future "why won't this load" moment does not mean solving the exact same mystery from scratch again.

## Document Findings, Actions, Outcomes, and Lessons Learned

This is the step I skipped entirely at the time, and writing about it now is effectively doing it late. What the problem was, what caused it, what fixed it, and what I would check first next time a similar symptom shows up — all of that is exactly what this final step asks for, and all of it was genuinely worth having written down the first time rather than relying on memory.

## Why the Order Actually Matters

Looking back, the useful part of this methodology is not any single step — it is that skipping ahead is exactly how troubleshooting goes wrong. Jumping straight to a fix without confirming the theory risks fixing the wrong thing. Fixing something without verifying it afterward risks assuming success that never actually happened. Following the steps in order, even without naming them at the time, was the entire reason that fault got found quickly rather than through random guessing.
