---
title: "The Network Ports Worth Memorising — and Why I Could Never Remember Them as Numbers"
date: 2026-06-02T00:00:00+01:00
draft: false
description: "Ports and protocols are exam staples and a memorisation nightmare. Here is how each one finally stuck for me — by knowing the job it does — and why a security mindset cares which is which."
tags: ["networking", "security", "ports", "protocols", "comptia", "soc", "homelab"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

The list of ports and protocols you have to know for Network+ is one of the most memorisation-heavy parts of the exam. Dozens of protocols, each with a number, and you are expected to recall them on demand. When I first tried to learn them as a flat list — FTP 21, SSH 22, DNS 53, on and on — none of it stuck. They were just numbers next to acronyms, and within a day they were gone.

What changed it was the same thing that changed TCP/IP for me. I stopped trying to memorise the numbers and started learning what each protocol actually does. Once a port was attached to a real job — something I had used, configured, or broken — it stopped being a number and became a thing with a purpose. This post is how I made them stick, grouped the way that worked for me, with the security angle that turned out to matter just as much as the numbers.

## Why Ports Exist at All

Before the list, the concept. In my last post I described IP as a road network getting data to the right building. A port is what gets it to the right room inside that building.

A single server can run many services at once — a website, an email service, a database. They all share one IP address. The port number is how the machine knows which service an incoming request is for. Port 80 means "the web server." Port 22 means "the SSH service." The IP gets you to the machine; the port gets you to the specific service running on it.

That was the first thing that made ports feel real rather than arbitrary. They are not random numbers. They are addresses for services.

## The Ports I Learned by Using Them

These are the ones that stuck first, because I had hands-on reasons to remember them.

**HTTP (80) and HTTPS (443)** became real when I started building websites. HTTP on port 80 is unencrypted web traffic. HTTPS on port 443 is the encrypted version. Building my own site and configuring a relative's meant these stopped being exam entries and became the difference between a site browsers trust and one they flag as "Not Secure." I now find it hard to forget 80 and 443 because I have watched what each one does in practice.

**DNS (53)** clicked through configuring domains. As I wrote about in an earlier post, DNS only became real to me when I had to set up records for a website and saw it translating names into addresses. Port 53 is where that happens. Once you have waited for a DNS change to propagate, the number sticks.

**SSH (22)** became concrete through Linux administration. Every time I watched or followed a tutorial on managing a Linux machine remotely, SSH on port 22 was the way in — an encrypted connection to a remote shell. It is the backbone of remote Linux work, and you remember it once you have actually used it to reach a machine.

**DHCP (67/68)** made sense once I understood how devices get their IP addresses automatically. Rather than setting a static address on every device, DHCP hands them out. It uses ports 67 and 68 — the server listens on 67, the client on 68. Understanding it as the quiet service that assigns addresses in the background made the numbers stick.

**HTTP again, on port 80, through Podman.** This is where it got hands-on. I ran an Apache web server inside a Podman container and had to publish port 80 to my local machine to view the page in a browser. Suddenly "port 80 is HTTP" was not a fact I had read — it was the thing I had just typed into a command to make a web page appear. That single exercise taught me more about what a port is than any flashcard.

**Port 8080 and a lesson in what "secure" actually means.** I also ran a local large language model with OpenWebUI in a Podman container. Its interface runs on port 8080 by default, over plain HTTP — it is not encrypted. Rather than expose that port to my whole network, I bound it to localhost, so only my own machine could reach it. This taught me a distinction worth being precise about: limiting the port to localhost did not make the connection encrypted, it just meant nothing outside my machine could reach it. The traffic was still plain HTTP. The security came from limiting exposure, not from encryption. Understanding where one ends and the other begins matters when you are thinking about risk.

## The Security Story — Secure vs Insecure Pairs

Once the protocols felt real, a pattern jumped out that I now cannot unsee. Many protocols come in pairs — an older insecure version and a newer encrypted one. This is where ports stop being trivia and start being a security signal.

- **Telnet (23) vs SSH (22)** — Telnet sends everything, including passwords, in plaintext. SSH does the same job encrypted. There is almost no reason to use Telnet today.
- **HTTP (80) vs HTTPS (443)** — plaintext web versus encrypted web.
- **FTP (20/21) vs SFTP (22)** — plaintext file transfer versus secure file transfer over SSH.
- **SMTP (25) vs SMTPS (587)** — plaintext email submission versus encrypted.
- **LDAP (389) vs LDAPS (636)** — plaintext directory queries versus encrypted.

The thing that genuinely shifted for me is that I now look at the insecure versions with suspicion. Why would you send credentials over Telnet in plaintext when SSH exists and does the same thing safely? These are not just exam topics — they show up in real logs, real alerts, real decisions. Plaintext protocols on a network are a red flag. Telnet traffic where you would expect SSH, or plain HTTP where sensitive data is involved, is exactly the kind of thing worth investigating. The port number tells you not just what the traffic is, but whether it should be there at all.

Two ports are worth knowing as common security concerns. **RDP (3389)**, the Remote Desktop Protocol, gives graphical remote access to a Windows machine. That makes it genuinely useful for remote work and administration, but it also means that if it is exposed to the internet with weak or default credentials, an attacker can attempt to brute-force their way in and gain full control of the machine. It is a port that should only ever be reachable from trusted sources. **SMB (445)**, used for Windows file and printer sharing, has appeared at the centre of some significant malware outbreaks — most notably WannaCry in 2017, which exploited an SMB vulnerability to spread rapidly from machine to machine across networks. Both ports have legitimate uses, but seeing either exposed where it should not be is worth investigating.

## The Full List for the Exam

Here is the complete set from the Network+ objectives, grouped by what they do so they are easier to hold in your head than a flat list. Each one is a service, doing a job, on a number.

**File transfer**
- FTP — 20/21 — the File Transfer Protocol, used to move files between machines. It sends everything, including login credentials, in plaintext, which is why it has largely been replaced by secure alternatives. Uses two ports: 21 for control commands and 20 for the actual data.
- SFTP — 22 — Secure File Transfer Protocol, which does the same job as FTP but runs over SSH, so the transfer is encrypted. It uses port 22 because it is effectively file transfer carried inside an SSH connection.
- TFTP — 69 — Trivial File Transfer Protocol, a stripped-down, lightweight version with no authentication. Commonly used on local networks for simple jobs like pushing configuration files to routers and switches or booting devices over the network.

**Remote access**
- SSH — 22 — Secure Shell, an encrypted connection to a remote machine's command line. The standard way to administer a Linux server you are not sitting in front of.
- Telnet — 23 — the older, unencrypted way of doing what SSH does. It sends everything in plaintext, including passwords, so it is considered obsolete and unsafe for real use.
- RDP — 3389 — Remote Desktop Protocol, which gives you the full graphical desktop of a remote Windows machine, as though you were sitting at it.

**Web**
- HTTP — 80 — Hypertext Transfer Protocol, the protocol for serving web pages. Unencrypted, so anything sent over it can be read in transit.
- HTTPS — 443 — the encrypted version of HTTP, secured with TLS. This is what the padlock in your browser represents, and what nearly all of the modern web uses.

**Email**
- SMTP — 25 — Simple Mail Transfer Protocol, used by mail servers to send and relay email. Port 25 is the traditional, unencrypted port.
- SMTPS — 587 — the modern port for submitting outgoing mail securely, with encryption applied so credentials and message contents are protected in transit.

**Name and address services**
- DNS — 53 — the Domain Name System, which translates human-readable names like example.com into the IP addresses machines actually use to find each other. Without it you would have to remember an IP address for every site you visit.
- DHCP — 67/68 — Dynamic Host Configuration Protocol, which automatically hands out IP addresses to devices when they join a network, so you do not have to configure each one by hand. The server listens on port 67 and the client on port 68.

**Directory services**
- LDAP — 389 — Lightweight Directory Access Protocol. A directory is a central database of an organisation's users, computers, and groups — most commonly Microsoft's Active Directory. LDAP is the protocol used to query and authenticate against that directory, for example when you log into a work computer and it checks your username and password against the central system. Unencrypted on port 389.
- LDAPS — 636 — the same directory queries as LDAP, but encrypted, so usernames and passwords are not sent in plaintext across the network.

**Management, time, and logging**
- NTP — 123 — Network Time Protocol, which keeps the clocks on all the devices in a network synchronised. This matters more than it sounds — accurate timestamps are essential for security logs to line up across systems during an investigation.
- SNMP — 161/162 — Simple Network Management Protocol, used to monitor and manage network devices like routers and switches, gathering information such as whether a device is up and how it is performing.
- Syslog — 514 — a standard for sending log messages from many devices to one central logging server. Central to security monitoring, because it puts all the logs in one place where they can be searched and correlated.

**File sharing**
- SMB — 445 — Server Message Block, the protocol behind Windows file and printer sharing. It is how you access shared folders and network drives in a Windows environment.

**Database**
- SQL Server — 1433 — the port used by Microsoft SQL Server for database traffic. If an application needs to talk to a Microsoft database, this is usually the port it uses.

**Voice**
- SIP — 5060/5061 — Session Initiation Protocol, which sets up and tears down voice and video calls in internet telephony (VoIP). Port 5060 is unencrypted, 5061 is the encrypted version.

## Why This Matters Beyond the Exam

Memorising ports for an exam is one thing. Understanding them is what makes the rest of the work possible. A SOC analyst reads ports constantly — they are how you know what a connection is for, whether it belongs, and whether it should worry you. A firewall rule is a decision about which ports to allow. An alert about unexpected traffic is often an alert about a port that should not be open.

I could not learn these as numbers. I learned them as jobs — DNS resolving the names for a website I built, SSH reaching into a Linux box, port 80 publishing a container so I could see a web page, port 8080 taught me the difference between hidden and encrypted. Each one is attached to something I actually did, and that is why they finally stuck.

If you are staring at the same list of numbers I was, my advice is to stop memorising and start using. Spin up a container, build a small site, set up SSH on a machine you own. The numbers attach themselves to the experience, which makes them far easier to hold on to. They can still fade if you never touch them again — if you do not use it, you lose it — but it is much easier to recall something you have done than something you only read once.
