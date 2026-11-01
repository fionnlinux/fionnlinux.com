---
title: "Resources"
date: 2026-05-26
draft: false
description: "Free tools, docs and learning resources for Linux, CompTIA study, IT support, cloud and scripting: what I use and what I am working through next."
showDate: false
showReadingTime: false
showTableOfContents: false
showWordCount: false
showComments: false
showAuthor: false
showHero: true
heroStyle: "big"
imagePosition: "center"
---

{{< lead >}}
A curated list of free tools, documentation, and learning resources for Linux, IT support, cloud, and scripting. Some I use daily, some are reference material, and some are what I am working through next — all are worth knowing about.
{{< /lead >}}

---

## {{< icon "terminal" >}} Linux and Red Hat

### Documentation

- [Fedora Documentation](https://docs.fedoraproject.org) — official Fedora docs, well maintained and thorough. My first stop for anything Fedora specific.
- [AlmaLinux Wiki](https://wiki.almalinux.org) — the go-to reference for the RHEL-compatible ecosystem. Covers everything from installation to administration.
- [Red Hat Documentation](https://docs.redhat.com) — official RHEL documentation, the authoritative source for anything that matters in the enterprise Linux world.
- [Red Hat Developer Subscription](https://developers.redhat.com/products/rhel/download) — free RHEL subscription for individuals. The hands-on environment for RHCSA study.
- [Arch Wiki](https://wiki.archlinux.org) — the best Linux reference on the internet regardless of which distribution you use. If it is not here, it is probably not documented anywhere.
- [Podman Documentation](https://docs.podman.io) — rootless containers, daemonless architecture, and the right tool for Red Hat environments. Well documented and actively maintained.
- [Docker Documentation](https://docs.docker.com) — the container tool most tutorials and job postings assume. Worth knowing alongside Podman.

### Raspberry Pi

A family Raspberry Pi is on my shopping list, so this is where I will be starting.

- [Raspberry Pi Documentation](https://www.raspberrypi.com/documentation/) — the official docs, from getting started and installing an operating system to remote access over SSH, plus a Linux section for beginners.
- [Raspberry Pi Imager](https://www.raspberrypi.com/software/) — the official tool for putting an operating system onto an SD card.
- [Raspberry Pi Projects](https://projects.raspberrypi.org) — free step-by-step project tutorials from the Raspberry Pi Foundation. A good way to spend a first afternoon with a new Pi, and to work through with the kids.
- [The MagPi](https://magpi.raspberrypi.com) — the official Raspberry Pi magazine, with free PDFs of every issue. Packed with projects and tutorials.

### YouTube

- [Learn Linux TV](https://www.youtube.com/@LearnLinuxTV) — practical Linux tutorials with a consistent focus on real administration skills. Good for RHCSA relevant content.
- [Into the Terminal — Red Hat](https://www.youtube.com/@IntotheTerminal) — Red Hat administration, RHEL content, and enterprise Linux from the people who build it.

### Podcasts

- [Linux Matters](https://linuxmatters.sh) — one of my regulars. Linux news and discussion, with genuinely good conversation rather than just headlines.
- [Late Night Linux](https://latenightlinux.com) — my other regular. Linux and open source discussion worth your time, and more opinionated than most.
- [Hybrid Cloud Show](https://hybridcloudshow.com) — industry professionals talking public cloud, private cloud, and everything in between. Part of the Late Night Linux family.
- [2.5 Admins](https://2.5admins.com) — two sysadmins and a producer working through real-world administration topics. Practical and no-nonsense.
- [The Raspberry Pi Podcast](https://www.raspberrypi.com/news/welcome-to-the-raspberry-pi-podcast/) — the official Raspberry Pi podcast, covering hardware, software, and the community around it.

---

## {{< icon "bookmark" >}} Certifications and Study

### Courses and practice

- [Professor Messer](https://www.professormesser.com) — the standard for CompTIA exam preparation. Free video courses for A+, Network+, and Security+, plus daily pop quizzes and live study group replays. Use alongside the study guides.
- [CompTIA Exam Objectives](https://www.professormesser.com/objectives/) — a page linking to the official objectives for A+, Network+, and Security+. Every topic on the exam is listed, so it doubles as a study checklist.
- [BurningIceTech](https://www.youtube.com/@burningicetech) — CompTIA PBQ preparation. Pairs well with Professor Messer for the performance based questions that trip people up.
- [SubnettingPractice.com](https://subnettingpractice.com) — the most thorough free subnetting practice tool I have found. Used heavily during Network+ preparation.

---

## {{< icon "monitor" >}} IT Support and Windows

### Documentation

- [Windows Client Documentation](https://learn.microsoft.com/en-us/windows/resources/) — Microsoft's documentation for deploying, securing, and managing Windows. The reference for the everyday side of IT support.
- [Microsoft Intune Documentation](https://learn.microsoft.com/en-us/intune/) — managing devices and apps from the cloud. The hands-on side of the MD-102 exam.
- [MD-102 Study Guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/md-102) — Microsoft's official study guide for the Endpoint Administrator exam, with links to free learning paths.
- [PowerShell Documentation](https://learn.microsoft.com/en-us/powershell/) — the official docs, including the PowerShell 101 series for beginners. The Windows counterpart to Bash.

### Tools

- [Sysinternals](https://learn.microsoft.com/en-us/sysinternals/) — Microsoft's free troubleshooting tools, including Process Explorer, Process Monitor, and Autoruns. A staple for anyone supporting Windows machines.

---

## {{< icon "cloud" >}} Cloud

### Documentation

- [Microsoft Azure Documentation](https://learn.microsoft.com/en-us/azure/) — the official Azure docs. Well structured and the authoritative reference for everything Azure.
- [Microsoft Learn](https://learn.microsoft.com) — free structured learning paths for AZ-900 and AZ-104. The best free resource for Azure certification preparation.

### YouTube

- [Adam Marczak — Azure for Everyone](https://www.youtube.com/@AdamMarczakYT) — Azure fundamentals and certification content, well regarded for AZ-900 and AZ-104 preparation.

### Practice

- [Azure Free Account](https://azure.microsoft.com/free/) — a $200 credit for 30 days, plus free monthly amounts of popular services for 12 months and some that are always free. Set a budget alert before you build anything, because once you move to pay as you go, anything beyond the free amounts is billed.

---

## {{< icon "code" >}} Automation and Scripting

### Ansible

- [Ansible Documentation](https://docs.ansible.com) — the official docs are excellent. Start with the getting started guide and work through the playbook documentation systematically.
- [Jeff Geerling](https://www.youtube.com/@JeffGeerling) — Linux, homelab, and hardware projects. Ansible for DevOps is his book and his channel reflects the same depth.

### Bash

- [ShellCheck](https://www.shellcheck.net) — paste in a script and it points out mistakes and bad habits, with an explanation for each. I use it on my scripts.
- [LearnShell.org](https://www.learnshell.org) — interactive browser based shell scripting tutorial. Good for getting started without setting up a local environment.
- [The Bash Guide (Wooledge)](https://mywiki.wooledge.org/BashGuide) — the most thorough free Bash reference available. Go here once the basics make sense.
- [GNU Bash Manual](https://www.gnu.org/software/bash/manual/) — the official authoritative reference. Dense but complete.

### Python

- [futurecoder](https://futurecoder.io) — open source, interactive Python learning that starts from genuine zero. The best free resource for getting the fundamentals solid.
- [Python Documentation](https://docs.python.org/3/) — the authoritative reference. Once the basics are in place this becomes the first place to check.

---

## {{< icon "tool" >}} Tools and Site Building

### Version Control

- [GitHub Documentation](https://docs.github.com) — covers Git workflows, Actions, Pages, and everything else in the GitHub ecosystem. Clear and well maintained.
- [Pro Git Book](https://git-scm.com/book/en/v2) — free, comprehensive, and the standard Git reference. Worth reading properly rather than piecing it together from scattered tutorials.

### Editors

- [Vim Cheat Sheet](https://vim.rtorr.com) — a handy quick reference for Vim keybindings. Worth bookmarking early on, and pair it with vimtutor.

### Site Building

- [Hugo Documentation](https://gohugo.io/documentation/) — the official Hugo docs, well structured and thorough. The functions and templates reference is particularly helpful.
- [Blowfish Theme Documentation](https://blowfish.page/docs/) — thorough documentation covering every configuration option. This site is built with Blowfish.

---

## {{< icon "home" >}} Tools I Use Daily

- **[Tor Browser](https://www.torproject.org)** — privacy focused browser that routes traffic through the Tor network. Used alongside Firefox for situations requiring stronger anonymity.
- **[Bitwarden](https://bitwarden.com)** — open source password manager. The single most impactful security tool most people are not using. Self-hostable if you want full control.
- **[Proton Mail](https://proton.me/mail)** — privacy respecting email with end to end encryption. My primary email provider via a custom domain.
- **[Proton VPN](https://protonvpn.com)** — open source VPN with a no logs policy. Part of the Proton ecosystem and audited independently.
- **[Proton Authenticator](https://proton.me/authenticator)** — open source 2FA app with end to end encrypted backup. Replaces Google Authenticator without the privacy trade-off.
- **[SimpleLogin](https://simplelogin.io)** — open source email alias service. Every account gets its own alias, compartmentalising exposure if a service is breached.
- **[NextDNS](https://nextdns.io)** — DNS based content filtering configured per device. Blocks trackers and malicious domains before a connection is made.
- **[Firefox](https://www.mozilla.org/firefox/)** — open source browser and my daily driver, hardened with uBlock Origin, Multi-Account Containers for session isolation, and telemetry disabled.
- **[Signal](https://signal.org)** — end to end encrypted messaging, open source, and the standard for secure communication.

---

This page reflects what I use and what I am working through right now. If something has genuinely helped you on a similar path, I'd like to know about it.

{{< button href="mailto:contact@mail.fionnlinux.com" >}}
Get in touch {{< icon "mail" >}}
{{< /button >}}