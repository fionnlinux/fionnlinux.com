---
title: "When the Cable Itself Is the Problem — Troubleshooting Cabling and Physical Interfaces"
date: 2026-07-22T00:00:00+01:00
draft: false
description: "A single intermittent port fault, worked through cable, interface, and hardware in turn, grounded in Cat5 crimping and termination from an electrical apprenticeship."
tags: ["networking", "comptia", "troubleshooting"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

I have actually run Cat5 cable myself, back during my electrical apprenticeship, installing it through plastic trunking into a new office for the computers there. I think it ran separately from the electrical cabling to stop interference, but it has been long enough now that I honestly do not remember for sure anymore. What I remember clearly is crimping the ends in a specific wire order because that is what I was shown to do, without really understanding at the time why the order mattered or what would actually happen if it was wrong. Coming back to it now, that gap makes a lot more sense — getting the order wrong does not just fail outright, it produces a specific, diagnosable symptom, and this objective is really about learning to recognise that symptom rather than memorising a list of possible faults.

Picture a small office switch, with one particular port serving a workstation that keeps dropping its connection and running slowly, while every other port on the same switch is fine. That single fault is enough to walk through every layer this objective actually covers.

## Ruling Out the Cable

The first place to look is the cable itself, since a cable problem can produce almost any symptom depending on exactly what is wrong with it.

**Incorrect cable** is the first thing worth confirming — is this actually Cat 5, 6, 7, or 8, and does it match what the connection needs? In this case it checks out: proper Cat6, correctly rated for the speed being asked of it. **Single-mode vs multimode** applies to fibre rather than copper — single-mode fibre carries light down a single, narrow path and is built for long distances, while multimode carries light down multiple paths at once and is meant for shorter runs. Neither applies here at all, since this is a straightforward copper run to a workstation rather than fibre. **STP vs UTP** — shielded twisted pair versus unshielded twisted pair — refers to whether the cable has an extra layer of shielding around the wires to block outside electrical interference. With no heavy electrical equipment nearby, standard unshielded cable is a reasonable choice and not the cause here.

**Signal degradation** is next. **Crosstalk** is interference between the different pairs of wires inside the same cable, one pair's signal bleeding into another. **Interference** is the same basic problem but from outside the cable entirely — nearby electrical equipment or other cabling affecting the signal. Both would usually show up if the cable ran alongside something disruptive — fluorescent lighting, motors, other cabling bundled too tightly — but a visual check of the run shows nothing obviously wrong there. **Attenuation** is simply the signal weakening the further it travels down the cable, which is why every cable category has a maximum rated distance. It would only be a concern near that maximum, and this run is well within range. None of these fit.

**Improper termination** is where the actual fault turns out to be. Pulling the connector for inspection, one pair is untwisted further back than it should be, right at the crimp itself. Each pair inside a cable is twisted specifically to cancel out electrical interference between its two wires, and untwisting it removes that cancellation at exactly the point in the run most exposed to picking up noise. The result is not a dead link — the connection still comes up — but it explains a fault that is intermittent and gets worse under load rather than failing outright. **TX/RX transposed** is ruled out at the same time. TX and RX refer to the transmit and receive lines — the pair of wires a device sends data on, and the pair it listens for incoming data on. Transposed means those two are swapped somewhere in the connection, which is ruled out here because the link negotiates and passes traffic in both directions; a genuine transmit/receive swap would prevent that entirely rather than just degrading it.

## Confirming It Through the Interface

Before touching anything, the switch itself is worth checking, because it was already reporting exactly this.

**Increasing interface counters** on that specific port show a steadily climbing count of **CRC errors** — cyclic redundancy check failures, meaning frames are arriving corrupted. That lines up precisely with the termination fault just found: interference introduced right at the connector is exactly the kind of thing that corrupts frames without stopping the link outright. **Runts** and **giants** are both flat at zero — a runt is a frame shorter than the minimum valid size, and a giant is one larger than the maximum, and either would usually point to a workstation network card generating malformed frames rather than a cabling fault. With both at zero, that explanation is ruled out. **Drops** are elevated too — a drop is a frame the interface actually received but discarded rather than passing on, commonly because it failed a check like CRC. That is consistent with the switch discarding corrupted frames rather than passing them onward.

**Port status** is checked as well, mostly to rule it out. **Error disabled** would mean the switch itself detected a problem and automatically shut the port down — not the case here, since the port is still up. **Administratively down** would mean someone had deliberately turned the port off through configuration, which nobody had done. **Suspended** typically applies to a port temporarily held back from a VLAN or trunk it is trying to join, which does not apply to an ordinary access port like this one. None of the three fit — the port is fully up and passing traffic, just badly, which points to a physical fault rather than anything configured at the switch.

## Hardware, Considered and Set Aside

For completeness, hardware is worth a mention even though it is not the cause here. **PoE** — Power over Ethernet, a switch supplying electrical power to a device over the same cable carrying its data — is not relevant to this port at all, since it is not powering anything. Had it been, two faults would be worth checking: **power budget exceeded**, where a switch is asked to power more devices than its total PoE capacity actually allows, and **incorrect standard**, where the powered device and the switch simply do not agree on which PoE standard is in use. Neither applies here.

**Transceivers** — the small modules that plug into a switch to provide a specific type of physical connection, commonly used for fibre — are not involved either, since this is a plain copper connection to a workstation. Had a transceiver been in the path, a **mismatch** (the transceiver type not matching what is on the other end, such as single-mode paired with multimode) or a **signal strength** problem (the transceiver producing or receiving a weaker signal than it should) would be the two things worth checking. Hardware was quick to rule out here specifically because this port carries neither power nor fibre. A PoE camera or a fibre uplink would not get dismissed this easily — on those, if the cable and interface both checked out clean, the power budget or the transceiver itself would be the next real thing to actually investigate.

## Resolution

The fix is straightforward once the cause is confirmed: re-terminate the connector properly, keeping each pair twisted as close to the contact points as the standard allows. Reconnected, the CRC error count stops climbing, and the port holds a stable connection under the same load that was causing drops before.

## What This Actually Showed

Three places to check, and an order that mattered: the cable first, because it can mimic almost any other symptom; the interface next, because the switch was already reporting the fault before anything was touched; hardware last, ruled out quickly once it clearly did not apply. The CRC errors did not just confirm the fault — they pointed to termination specifically, long before the connector was ever pulled apart to look.
