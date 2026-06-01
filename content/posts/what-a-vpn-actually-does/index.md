---
title: "What a VPN Actually Does — and the Three Jobs I Use One For"
date: 2026-05-31T00:00:00+01:00
draft: false
description: "VPNs are sold as magic privacy boxes. The reality is more specific, more useful, and worth understanding properly."
tags: ["security", "networking", "vpn", "privacy", "comptia", "open-source", "homelab"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

My first encounter with VPNs had nothing to do with security. Like a lot of people, I came across them as a way to watch content from another country — making it look as though I was somewhere I was not. That was the whole of my understanding for a long time. A VPN was a thing that changed where the internet thought you were.

It was only when I started my CompTIA studies that I realised I did not actually understand what was happening underneath. I could describe what a VPN did from the outside, but not how, or why it mattered for security. And when I started trying to take charge of my privacy online and moved my email over to Proton, a VPN came bundled into the subscription I was already paying for. That gave me every reason to learn it properly — not just for the exam, but to get real use out of a tool I already had.

What I found is that "VPN" is a single word covering at least three quite different jobs. Conflating them is where most of the confusion comes from. This post is my attempt to explain what a VPN actually does, the three distinct things I use one for, and just as importantly, what a VPN does not do — because that last part is where most of the marketing falls apart.

## What a VPN Actually Is

VPN stands for Virtual Private Network. Strip away the marketing and it is a simple idea: an encrypted tunnel between your device and a server somewhere else.

Normally when you connect to the internet, your traffic goes from your device, through your router, to your internet service provider, and out to whatever website or service you are using. Everyone along that path can see something. Your internet service provider — your ISP — can see which sites you connect to. Whoever runs the network you are on, whether that is a coffee shop, an airport, or your workplace, can potentially see the same.

A VPN changes the first part of that journey. Instead of your traffic going straight out to the internet, it first travels through an encrypted tunnel to a VPN server. From there it goes out to the wider internet. Two things change as a result.

First, anyone watching your local connection — your ISP, the network owner — can no longer see what you are doing. They can see that you are connected to a VPN server, and that is all. The contents and destinations of your traffic are inside the encrypted tunnel.

Second, the websites you visit no longer see your real IP address. They see the VPN server's address instead. As far as they are concerned, your traffic is coming from wherever that server is located.

That second point is what makes the geo-content trick work. If the VPN server is in another country, services you connect to think you are in that country. That was my entry point, and it is most people's. But it is genuinely the least interesting thing a VPN does.

## The Tunnel — How the Encryption Works

The encrypted tunnel is worth taking a moment to fully understand, because it is the core of everything.

When your VPN connects, your device and the VPN server perform a handshake. They agree on encryption keys that only the two of them know. From that point on, everything your device sends is encrypted before it leaves, travels across the internet as unreadable scrambled data, and is only decrypted once it reaches the VPN server. The reverse happens on the way back.

Anyone intercepting the traffic in between sees noise. They know data is moving between you and a VPN server, but they cannot read it. That is the whole point of the tunnel — it makes the local section of your journey private, even on a network you do not trust or control.

## Protocol vs Service — A Distinction Worth Knowing

This is the part that tripped me up early, and it is exactly the kind of thing Network+ expects you to understand, so it is worth making explicit.

There is a difference between the VPN protocol and the VPN service.

The protocol is the technical method used to build the encrypted tunnel — the rules for how the handshake happens, what encryption is used, and how data is wrapped up and sent. Common protocols include WireGuard, OpenVPN, and IKEv2. WireGuard is the newest of these, dating from 2016, and has become the modern favourite because it is fast, has a small codebase of roughly 4,000 lines that is far easier to audit than OpenVPN's tens of thousands, and is simpler to configure than the older options.

The service is the company providing the VPN servers you connect to. Proton VPN is a service. So are the many others advertised everywhere.

The reason the distinction matters is that they are independent. Proton VPN uses WireGuard as one of its protocols, but WireGuard is not Proton — it is an open protocol that anyone can use, including in a setup you build entirely yourself with no company involved at all. I will come back to that, because it is exactly how my future home network plan works and is a long-term goal for both learning and self-hosting.

When people say "I use a VPN," they almost always mean they subscribe to a service provided by a company. But the protocol is the actual technology underneath. Knowing they are separate things is what made the different roles a VPN can play click into place for me.

## Job One — A Commercial Privacy VPN (Proton VPN)

This is the job most people mean. A commercial VPN service routes your daily traffic through their servers, hiding it from your local network and your ISP, and presenting a different IP to the wider internet.

I use Proton VPN for this. The honest reason I chose it is not that I exhaustively compared every provider — it came bundled with the Proton subscription I was already paying for after moving off Gmail, as I wrote about in an earlier post. Rather than leave it unused, I made a point of understanding it and folding it into daily use. It was the convenient and cost effective choice, and it happens to be a genuinely good one.

What makes a privacy VPN trustworthy comes down to a few things, and the reason I am comfortable recommending Proton is that it does well on all of them.

The first is a no-logs policy — the provider does not keep records of what you do — ideally backed by independent audits rather than just a promise. This is where most providers ask you to take their word for it. Proton does not. Its no-logs policy has been independently audited four years running by Securitum, a European security firm, with auditors physically inspecting the live server infrastructure in Zurich rather than just reading documentation. The reports are published in full on Proton's site for anyone to read. A promise is one thing; a promise that has been checked on-site by outside experts four times over is another.

The second is open source clients, so the code doing the work can be inspected rather than trusted blindly. All of Proton's apps are open source. You do not have to take their word for what the software is doing — the code is there to read, and people do read it.

The third is jurisdiction. Proton is based in Switzerland, which currently places no logging requirements on VPN providers. There is a real-world test of this too. In a 2019 Swiss legal case, authorities approached Proton for VPN user data and Proton had no logs to hand over, because the infrastructure was not built to keep them. That is the strongest kind of proof — not a claim about what would happen, but what actually did.

I want to be honest rather than write an advertisement, so the caveats are worth noting. Proton uses full-disk encryption rather than the RAM-only servers some competitors use, and there are proposed changes to Swiss surveillance law that privacy watchers are keeping an eye on. Neither undermines the core picture today, but a security mindset means holding even the tools you trust to ongoing scrutiny rather than assuming they are settled forever.

But — and this matters — using a commercial VPN does not make your traffic private in some absolute sense. It moves the trust. Here is the part that confused me for a while, and it is worth being precise about. When your traffic reaches the VPN server, the tunnel encryption is stripped away so the traffic can be sent on to its destination. That means the provider can see your traffic in the same way your ISP otherwise would — which sites and services you are connecting to. This is different from Proton Mail or Proton Drive, where end to end encryption means Proton genuinely cannot read your content because they do not hold the keys. A VPN does not work that way. The provider decrypts the tunnel at their end, so they are not blind to your traffic.

There is an important caveat though, and it is the same point I make about HTTPS later. While the provider can see which sites you connect to, they cannot see the contents of anything protected by HTTPS. If you log into your bank over the VPN, Proton can see that you connected to your bank, but not your password or what you did there, because HTTPS encrypts that content end to end between your browser and the bank. So the provider sees destinations, not the sensitive contents of secured connections.

The upshot is that you are not eliminating the need to trust someone. You are choosing to trust a provider whose entire business is privacy, instead of an ISP whose business is not. That is a reasonable trade, but it is a trade, not magic. I will expand on the trust point in the "what a VPN is not" section.

## Job Two — Reaching My Own Home Network From Anywhere (WireGuard)

This is the job that genuinely excited me once I understood it, and it is a planned part of my homelab rather than something I have built yet. This section describes where I am heading, not something I am already running — it is a long-term goal.

The aim is to be able to reach my home network securely from anywhere in the world. Say I am away from home and I want to check on a service running in my homelab, or access something on my home network as though I were sitting in the house. A VPN makes that possible, but in a completely different way to the commercial service above.

Here the encrypted tunnel does not go to a company's server. It goes to my own router at home. My plan is to run WireGuard as a plugin on an OPNsense firewall — a dedicated Protectli device that will sit at the centre of my home network. WireGuard on that box becomes the endpoint of the tunnel.

When I am out and about, my phone or laptop running a WireGuard client establishes an encrypted tunnel back to that OPNsense box at home. Once that tunnel is up, my device behaves as if it is physically on my home network. I can reach devices and services by their local addresses, monitor or control things, all over an encrypted connection that nobody in between can read.

The mechanism is the same encrypted tunnel as before — the difference is entirely about where the far end of the tunnel sits. In a commercial VPN, the far end is the provider's server and the purpose is to get out to the internet privately. In this home setup, the far end is my own router and the purpose is to get in to my own network securely. Same technology, opposite direction, completely different job.

This is why the protocol versus service distinction matters so much. There is no company involved in this setup at all. It is just WireGuard, the open protocol, running on hardware I own, talking to a client on a device I own. No subscription, no third party, no trust transferred to anyone. That is a genuinely different and powerful use of the same underlying idea, and it is exactly the kind of thing a homelab is for.

## Job Three — Staying Safe on Untrusted Networks

This is the job I use Proton VPN for most in practice, and it is the most immediately practical for anyone.

I connect to a shared network at work — one used by everyone, that I do not control and cannot see the configuration of. That is the textbook definition of an untrusted network. I do not know who else is on it or what they are running. The same applies to any public WiFi — cafes, airports, hotels, anywhere the network is shared and out of your hands.

The risk on these networks is that other parties on the same network, or whoever runs it, may be able to observe traffic that is not otherwise protected. This used to be a serious and easily exploited problem. Years ago, much of the web ran on plain HTTP, which sends everything in readable plaintext. Anyone on the same WiFi could run a packet capture tool and read usernames, passwords, and page contents straight off the network. Around 2010 a browser extension called Firesheep made hijacking other people's logged-in sessions on open WiFi almost trivial, and it was one of the things that pushed the whole web towards encryption.

That specific attack is far less of a threat today, and it is worth being honest about why rather than overstating the danger. Most of the web now uses HTTPS, which encrypts the contents of your connection between your browser and the website. So even if someone captures your traffic on shared WiFi, the contents — passwords, messages, form data — are encrypted and unreadable. Properly implemented modern HTTPS cannot be decrypted just by passively listening to the network. The classic read-your-password-off-the-WiFi attack mostly does not work against the HTTPS sites that now make up the majority of the web.

So why still use a VPN here? Because HTTPS does not cover everything. Even with HTTPS, anyone watching the network can still see which sites you are connecting to — the destinations leak through the connection setup and through DNS lookups, even when the contents do not. HTTPS also only protects your browser traffic to sites that implement it correctly. Any app that is misconfigured, any service still using plain HTTP, any connection that gets downgraded — those are still exposed. A VPN closes all of that in one move. With the tunnel active, everything leaving my device is encrypted before it touches the shared network, browser or not, and the local network can no longer see even which sites I am visiting. Whoever else is on that network sees only encrypted traffic to a VPN server.

The honest framing is this. The dramatic "hackers will steal your password on public WiFi" line is mostly a relic of the pre-HTTPS era. But a VPN on an untrusted network still does two things HTTPS alone does not — it hides your destinations from the local network, and it protects all your traffic rather than just well-behaved browser connections. That is a real benefit, just not the lurid one the marketing reaches for.

This is still the use case I would recommend to anyone, regardless of how technical they are. If you regularly use WiFi you do not control, a VPN closes a set of gaps that HTTPS on its own leaves open.

## What a VPN Is Not

This is the section the marketing skips, and it is the most important one. A VPN is a specific tool that does specific things. It is not a force field.

**A VPN does not make you anonymous.** This is the biggest myth. A commercial VPN hides your traffic from your ISP and your local network, and hides your IP from the sites you visit. It does not make you anonymous. The VPN provider can see your traffic. The sites you log into still know who you are the moment you sign in. Browser fingerprinting, cookies, and accounts identify you regardless of your IP. If you genuinely need anonymity rather than privacy, that is what Tor is built for, and it works very differently. A VPN is privacy from specific observers, not anonymity from everyone.

**A VPN does not remove the need for trust — it moves it.** As I said earlier, the moment you route your traffic through a provider, you are trusting that provider with what your ISP used to see. A VPN does not remove the need to trust someone. It lets you choose who. With a reputable, audited, no-logs provider that is a good trade. With a free VPN of unknown origin, you may be handing your data to something far worse than your ISP. The trust does not disappear. It relocates.

**A VPN does not replace HTTPS — and HTTPS does not replace a VPN.** This one matters for anyone studying networking. Most websites now use HTTPS, which already encrypts the contents of your connection between your device and the website. So does a VPN make that redundant? No — they protect different things. HTTPS encrypts the content of your traffic to a specific site, but anyone watching your connection can still see which sites you are connecting to. A VPN hides those destinations from your local network and ISP. HTTPS protects the conversation; a VPN hides who you are talking to from local observers. They layer together rather than replace each other.

**A VPN does not make you unhackable.** It does not stop malware, it does not block phishing, and it does not protect you if you hand your password to a fake login page. It secures one specific thing — the network path between your device and the VPN endpoint. Everything else about staying safe still applies.

None of this makes a VPN less useful. It makes it useful in a defined way. Understanding the boundaries is what lets you use it well, instead of relying on it for things it was never going to do.

## Why This Sits at the Heart of Both Threads

I am writing this while working through CompTIA Network+, and VPNs are squarely on the syllabus — tunnelling, encryption, protocols, remote access. Writing it out like this is how I check that I actually understand it rather than just recognising the terms on an exam. If I can explain it clearly enough to teach it, I know it has gone in.

It belongs just as much on the security side. The untrusted-network use case is everyday practical security. The home network plan is the kind of project that moves you from using security tools to building your own infrastructure. And being honest about what a VPN cannot do is part of the same mindset — knowing the limits of a tool matters as much as knowing its strengths.

That is the thread running through all three jobs. A VPN is one encrypted tunnel doing whichever job you point it at. The skill is knowing which job you are asking for, and not expecting it to do the other two.
