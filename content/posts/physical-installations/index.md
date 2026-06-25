---
title: "Physical Installations — What a Network Actually Sits In"
date: 2026-06-21T00:00:00+01:00
draft: false
description: "IDFs, MDFs, rack considerations, cabling, power, and environmental factors — the physical side of a network that's easy to overlook."
tags: ["networking", "comptia"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

A lot of networking attention goes on configuration — VLANs, routing tables, wireless settings — but all of it runs on physical equipment sitting in a physical room. This objective is about that room: where the equipment lives, how it's cabled, how it's powered, and what environmental conditions it needs to keep working. None of this is about configuring a device. It's about understanding why these factors matter.

---

## Locations — IDF and MDF

In any building bigger than a small office, network equipment isn't all in one place. It's organised into distribution frames.

The **MDF (Main Distribution Frame)** is the central point — the room where the main network equipment lives, where external connections enter the building, and where the core switches and routers are typically located. Think of it as the hub everything else connects back to.

An **IDF (Intermediate Distribution Frame)** is a secondary distribution point, usually one per floor or one per section of a larger building. It connects back to the MDF and distributes the network out to the end devices on that floor. The reason IDFs exist rather than running every single cable straight back to the MDF is distance and practicality — Ethernet has cable length limits, and running every cable in a large building back to one central room isn't realistic.

So the structure is generally: MDF at the centre, IDFs distributed around the building, each one serving its local area and connecting back to the MDF.

---

## Rack considerations

**Rack size** refers to the physical dimensions of equipment racks, measured in rack units (U). A piece of equipment's height is described in U — a 1U switch, a 2U server, and so on — and the rack itself has a maximum number of U it can hold. Planning rack space means accounting for the equipment you have now and leaving room for what you'll add later.

**Port-side exhaust/intake** matters for airflow. Network equipment generates heat, and in a rack full of devices, that heat needs somewhere to go. Equipment is generally designed with a clear airflow direction — air pulled in one side and exhausted out the other. The important point is consistency: if equipment in the same rack exhausts in different directions, devices end up pulling in air that's already been heated by something else nearby, which undermines the cooling for the whole rack.

---

## Cabling

**Patch panels** are fixed panels in a rack where the building's permanent cabling terminates. Rather than running long cables directly from the IDF to every device, the wall and floor cabling lands on the patch panel, and short patch cables connect from the patch panel to the actual switch ports. This makes moves, additions, and changes simpler — you're working with short patch cables in the rack rather than rerouting building cabling every time something changes.

**Fibre distribution panels** serve the same purpose but for fibre optic cabling — terminating fibre runs in an organised way and protecting the delicate fibre connections from damage.

**Lockable** racks and rooms are a physical security consideration. Unrestricted physical access to network equipment is a real risk — someone with hands-on access to a switch or router can bypass a lot of network security entirely. Lockable racks and access-controlled rooms are the basic physical security control for any space holding network equipment.

---

## Power

**UPS (Uninterruptible Power Supply)** provides battery backup power, keeping equipment running through a short power outage and giving time for an orderly shutdown or for backup generators to kick in during a longer one. It's the difference between equipment dropping instantly when power fails and equipment staying up long enough to handle the situation gracefully.

**PDU (Power Distribution Unit)** is essentially a specialised power strip for a rack — distributing power from one or more sources to multiple devices in the rack, often with monitoring or remote management built in.

**Power load** is the total amount of power being drawn by everything in a rack or room. This matters because circuits and UPS systems have limits — adding more equipment without checking the existing load can overload a circuit and trip it, taking down everything on it. Knowing the load also tells you how long a UPS will actually last during an outage, since a heavier load drains the battery faster.

**Voltage** matters because equipment is built for specific voltage standards, and these vary by region — most of Europe and the UK run at around 230V, while North America runs at around 120V. Some equipment auto-detects and handles both, but getting it wrong can mean a device simply doesn't work, or in some cases is damaged.

---

## Environmental factors

**Temperature** — network equipment generates heat and has an operating temperature range it needs to stay within. Server rooms and data centres are cooled deliberately, not just for comfort, but because consistently running hot equipment shortens its lifespan and increases the risk of failure.

**Humidity** also needs to be controlled. Too much humidity risks condensation and corrosion on components; too little increases the risk of static electricity, which can damage sensitive electronics. Server rooms are kept within a controlled humidity range for this reason.

**Fire suppression** in a server room is different from a typical building. Standard water sprinklers would destroy electronic equipment even while putting out a fire, so server rooms typically use suppression systems designed not to damage electronics — clean agent systems that remove oxygen or heat from the fire without leaving residue or causing water damage.

I went into my workplace's MDF once and found out it uses an oxygen-removal suppression system. My first thought wasn't about the equipment — it was that if a fire broke out while someone was in there, the same system protecting the hardware could leave them struggling to breathe. It's an effective way to protect a room full of irreplaceable equipment, but it's a sobering thing to stand in once you know what it would actually do.

---

## Why this objective exists

None of this is about logging into a device and changing a setting. It's about understanding that a network's reliability depends on more than its configuration — it depends on the room it's in. A perfectly configured network can still go down because of a power failure, overheating, or simply because someone walked into an unlocked room and unplugged the wrong cable. These physical factors are the foundation everything else sits on.
