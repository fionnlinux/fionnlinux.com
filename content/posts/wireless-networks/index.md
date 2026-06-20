---
title: "Wireless Networks — Selecting and Configuring the Right Setup"
date: 2026-06-19T00:00:00+01:00
draft: false
description: "Frequencies, channels, SSIDs, encryption, guest networks and more — the wireless decisions worth understanding and when each one matters."
tags: ["networking", "comptia", "wireless", "security", "homelab"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

Wireless networking is one of those areas where most people have more hands-on experience than they realise. If you've ever logged into a router and poked around the settings, you've already been here. Studying for my A+ was what first got me into the admin panel properly — changing the SSID, reviewing the security settings, understanding what each option actually did. It's a good place to start, and this post picks up where that leaves off.

Each topic here is worth thinking about as a decision: given a particular situation, which option makes sense and why?

---

## Frequencies — 2.4GHz, 5GHz, and 6GHz

Wi-Fi operates on different frequency bands, and the choice between them is one of the first decisions when configuring a wireless network.

**2.4GHz** has a longer range and passes through walls and obstacles better than higher frequencies. The trade-off is that it's a crowded band — home appliances like microwaves use it, neighbouring Wi-Fi networks use it, and there are fewer non-overlapping channels available. Where you need coverage across a larger space or through several walls, 2.4GHz holds up better.

**5GHz** offers faster speeds and less interference, but the range is shorter and it struggles more with walls and solid objects. I tested this recently — router downstairs, trying to get a decent signal in the bedroom upstairs on 5GHz. The 2.4GHz connection was noticeably better in that situation. If you're sitting close to the access point, 5GHz is the faster option. Put a few walls between you and it, and that advantage shrinks quickly.

**6GHz** is the newest addition, introduced with Wi-Fi 6E. It offers even faster speeds and lower congestion than 5GHz, but the range is shorter still and fewer devices currently support it. It's the right choice where speed matters, devices are nearby, and you're operating in a dense environment where interference is a problem.

**Band steering** is a feature on access points (APs) that automatically pushes a capable device toward the better band for its current situation — usually 5GHz when the signal is strong enough, falling back to 2.4GHz when it isn't. Rather than the user having to choose, the AP manages it.

---

## Channels

Within each frequency band, the available spectrum is divided into channels. The problem is that neighbouring channels overlap — a device on channel 1 can interfere with a device on channel 2.

**Non-overlapping channels** are the ones far enough apart that they don't interfere with each other. On 2.4GHz there are only three: channels 1, 6, and 11. On 5GHz there are far more, which is part of why it handles congestion better.

**Channel width** determines how much spectrum a channel uses. A wider channel (80MHz or 160MHz) can carry more data, but takes up more of the band and is more likely to overlap with other networks. Narrower channels (20MHz or 40MHz) are more conservative but more reliable in busy environments.

**Regulatory impacts** — different countries regulate which channels and power levels are permitted. **802.11h** is the standard that handles this for the 5GHz band, adding features to automatically avoid interference with radar systems, which share that frequency range in some regions. It's not something you configure manually, but it's worth knowing why it exists.

---

## SSIDs — naming and identifying a network

The **SSID** (Service Set Identifier) is the network name — what appears in the list when you scan for Wi-Fi. Changing it away from the default was one of the first things I did when I got into my router settings. Default names often include the router model or ISP name, which tells anyone nearby exactly what hardware you're running — and default SSIDs frequently come paired with default passwords that are either printed on the router or easy to look up. Changing the SSID is a minor thing on its own; leaving the default password in place is a much bigger problem.

**BSSID** (Basic Service Set Identifier) is the MAC address of the access point itself. Where you have multiple access points on the same network, each has its own BSSID even though they share the same SSID. It's how devices distinguish between access points when roaming.

**ESSID** (Extended Service Set Identifier) is the name shared across multiple access points that are all part of the same network. Where a BSSID identifies one specific access point by its MAC address, an ESSID is the umbrella name that ties several access points together. When you walk around an office and stay connected to "CompanyWifi" without manually reconnecting, you're roaming between access points that share the same ESSID — each with its own BSSID underneath.

---

## Network types

Not all wireless networks are structured the same way.

**Infrastructure mode** is the standard setup: devices connect to a central access point, which connects to the rest of the network. Most home and business Wi-Fi works this way.

**Ad hoc** is a direct device-to-device connection with no access point in the middle. Less common now, but it still comes up as a concept.

**Point to point** links two fixed locations wirelessly — often used to bridge buildings without running cable between them.

**Mesh networks** use multiple nodes that all communicate with each other, extending coverage without the dead spots you get from a single access point. Each node passes traffic to the next. It's a common solution for larger homes or offices where one AP doesn't reach everywhere.

---

## Encryption — WPA2 and WPA3

This is where the security decision sits. When I set up my home network properly during A+ study, I made sure to select the strongest encryption option available at the time — I went for the best one on the list, which turned out to be the right approach.

**WPA2** (Wi-Fi Protected Access 2) has been the standard for years and is still widely used. It uses AES encryption and is considered solid for most home and business use.

**WPA3** is the current standard and improves on WPA2 in a meaningful way. With WPA2, an attacker can capture the initial handshake between a device and the access point — the exchange that happens when you connect — and take that data away to try password guesses against it offline, without staying on the network. WPA3 uses a different handshake method that prevents this. If the hardware supports it, WPA3 is worth enabling.

---

## Authentication — PSK vs. Enterprise

**Pre-shared key (PSK)** is the password model: everyone connecting to the network uses the same passphrase. It's straightforward and fine for home use, but in a business setting it has a significant limitation — if someone leaves, the only way to revoke their access is to change the password for everyone.

**Enterprise authentication** uses a RADIUS server to authenticate each user individually with their own credentials. Each person has their own login, access can be revoked per user, and the network can log who connected and when. More complex to set up, but the right answer for any environment where individual accountability matters.

---

## Guest networks and captive portals

A **guest network** is a separate SSID that gives visitors internet access without putting them on the same network as internal devices. At home, that might be a network for visitors or for IoT devices you don't fully trust. At work, it's how you give customers or contractors connectivity without exposing internal systems.

A **captive portal** is the login page that appears before a guest network grants access — the screen asking you to accept terms or enter a code. I use one at work, and it's effective but not without friction. Sometimes the session drops and you have to go through the portal again to reconnect, which gets tedious quickly. It does its job of controlling access, though it's a good example of how security measures and user experience don't always sit comfortably together.

---

## Antennas — omnidirectional vs. directional

**Omnidirectional** antennas broadcast in all directions equally, like a sphere of signal around the access point. They're the standard choice for general coverage — most home routers use them.

**Directional** antennas focus the signal in a specific direction, which extends the range in that direction at the expense of coverage elsewhere. They're the right choice for a point-to-point link between two buildings, or for reaching a specific area that an omnidirectional antenna can't cover adequately.

---

## Autonomous vs. lightweight access points

An **autonomous AP** (access point) is self-contained — it handles its own configuration and runs independently. That's fine for a single access point or a small setup where each one is managed individually.

A **lightweight AP** offloads most of its intelligence to a central **wireless controller**, which manages configuration, firmware, and policy across all the access points from one place. In an environment with dozens or hundreds of APs, managing each one individually isn't practical. The controller handles it all centrally, and the lightweight APs operate under its direction.

---

## Picking the right option

The pattern across all of this is the same as with switching: the right answer depends on the situation.

- Poor coverage through walls → 2.4GHz, or a mesh network
- Fast speeds for nearby devices → 5GHz or 6GHz
- Crowded environment with lots of neighbouring networks → check non-overlapping channels, consider 5GHz
- Devices capable of both bands → band steering to let the AP decide
- Home or small office network → PSK authentication, WPA3 if the hardware supports it
- Business network where individual accountability matters → Enterprise authentication with a RADIUS server
- Visitors need internet access without touching internal systems → guest network with a separate SSID
- Controlling when and how guests connect → captive portal on the guest network
- Broad coverage in all directions → omnidirectional antenna
- Linking two buildings wirelessly, or reaching a specific area → directional antenna
- Single or small number of access points → autonomous APs
- Large environment with many access points to manage centrally → lightweight APs with a wireless controller

Each option above exists because a particular situation called for it. Knowing what problem each feature solves is what makes the difference between recognising a term and knowing when to reach for it.

Now go and log into your router, change the default SSID, and set a strong password — if you haven't already. Hopefully you have.
