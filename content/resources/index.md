---
title: "Resources"
date: 2026-05-26
draft: false
showDate: false
showReadingTime: false
showTableOfContents: true
showWordCount: false
showComments: false
showAuthor: false
showHero: false
---

A curated list of the tools, documentation, and learning resources I actually use. Nothing here for the sake of a longer list.

---

## Linux

**Documentation**

- [Fedora Documentation](https://docs.fedoraproject.org) — official Fedora docs, well maintained and thorough. My first stop for anything Fedora specific.
- [Rocky Linux Documentation](https://docs.rockylinux.org) — the go-to reference for the RHEL-compatible ecosystem. Covers everything from installation to administration.
- [Red Hat Documentation](https://docs.redhat.com) — official RHEL documentation, the authoritative source for anything that matters in the enterprise Linux world.
- [Arch Wiki](https://wiki.archlinux.org) — the best Linux reference on the internet regardless of which distribution you use. If it is not here, it is probably not documented anywhere.

**YouTube**

- [Learn Linux TV](https://www.youtube.com/@LearnLinuxTV) — practical Linux tutorials with a consistent focus on real administration skills. Good for RHCSA relevant content.
- [Into the Terminal — Red Hat](https://www.youtube.com/@IntotheTerminal) — Red Hat administration, RHEL content, and enterprise Linux from the people who build it.
- [Jeff Geerling](https://www.youtube.com/@JeffGeerling) — Linux, homelab, and hardware projects. Ansible for DevOps is his book and his channel reflects the same depth.
- [Red Hat](https://www.youtube.com/@redhat) — enterprise Linux, RHEL releases, and community content straight from Red Hat.
- [Fedora Project](https://www.youtube.com/@FedoraProject) — Fedora releases, features, and community updates.

**Podcasts**

- [Linux Matters](https://linuxmatters.sh) — Linux news and discussion. Genuinely good conversation rather than just headlines.
- [Late Night Linux](https://latenightlinux.com) — Linux and open source discussion worth your time. More opinionated than most.

---

## Security

**Documentation and Platforms**

- [Wazuh](https://documentation.wazuh.com) — open source SIEM documentation. Comprehensive and well structured. Essential once you are running it in a homelab.
- [MITRE ATT&CK](https://attack.mitre.org) — the definitive framework for understanding adversary tactics and techniques. Every SOC analyst needs to know this.
- [LetsDefend](https://letsdefend.io) — hands on SOC analyst training with realistic alert investigations. The closest thing to real SOC work outside of a job.

**YouTube**

- [MyDFIR](https://www.youtube.com/@MyDFIR) — SOC analyst content and blue team skills. Practical and directly relevant to entry level security roles.

---

## Networking

**Documentation**

- [OPNsense Documentation](https://docs.opnsense.org) — open source firewall and routing platform. Clear and well organised for both beginners and experienced administrators.
- [Wireshark Documentation](https://www.wireshark.org/docs/) — essential reference for network traffic analysis. Pairs well with any networking certification study.

**Practice Tools**

- [SubnettingPractice.com](https://subnettingpractice.com) — the most thorough free subnetting practice tool I have found. Used heavily during Network+ preparation.

---

## Certifications and Study

**YouTube**

- [Professor Messer](https://www.youtube.com/@professormesser) — the standard for CompTIA exam preparation. Free video courses for A+, Network+, and Security+. Use alongside the study guides.
- [BurningIceTech](https://www.youtube.com/@burningicetech) — CompTIA PBQ preparation. Pairs well with Professor Messer for the performance based questions that trip people up.

---

## Scripting and Automation

**Python**

- [futurecoder](https://futurecoder.io) — open source, interactive Python learning that starts from genuine zero. The best free resource for getting the fundamentals solid.
- [Python Documentation](https://docs.python.org/3/) — the authoritative reference. Once the basics are in place this becomes the first place to check.
- [W3Schools Python Reference](https://www.w3schools.com/python/) — quick reference when you need a syntax reminder without reading the full docs.

**Bash**

- [LearnShell.org](https://www.learnshell.org) — interactive browser based shell scripting tutorial. Good for getting started without setting up a local environment.
- [The Bash Guide (Wooledge)](https://mywiki.wooledge.org/BashGuide) — the most thorough free Bash reference available. Go here once the basics make sense.
- [GNU Bash Manual](https://www.gnu.org/software/bash/manual/) — the official authoritative reference. Dense but complete.

**Ansible**

- [Ansible Documentation](https://docs.ansible.com) — the official docs are excellent. Start with the getting started guide and work through the playbook documentation systematically.

**YouTube**

- [The Ansible Playbook](https://www.youtube.com/@TheAnsiblePlaybook) — practical Ansible tutorials focused on real administration use cases.

---

## Containers

**Documentation**

- [Podman Documentation](https://docs.podman.io) — rootless containers, daemonless architecture, and the right tool for Red Hat environments. Well documented and actively maintained.

---

## Development Tools

**Version Control**

- [GitHub Documentation](https://docs.github.com) — covers Git workflows, Actions, Pages, and everything else in the GitHub ecosystem. Clear and well maintained.
- [Pro Git Book](https://git-scm.com/book/en/v2) — free, comprehensive, and the standard Git reference. Read this properly rather than picking up Git habits from Stack Overflow.

**Editors**

- [Vim Documentation](https://www.vim.org/docs.php) — official Vim docs. Start with vimtutor before coming here.
- [Vim Cheat Sheet](https://vim.rtorr.com) — the most useful quick reference for Vim keybindings. Worth bookmarking early on.

---

## Site Building

**Documentation**

- [Hugo Documentation](https://gohugo.io/documentation/) — the official Hugo docs, well structured and thorough. The functions and templates reference is particularly useful.
- [Blowfish Theme Documentation](https://blowfish.page/docs/) — thorough documentation covering every configuration option. This site is built with Blowfish.

**YouTube**

- [CloudCannon — Hugo](https://www.youtube.com/@cloudcannon) — Hugo tutorials and static site content from people who work with it professionally.
- [Mike Dane](https://www.youtube.com/@GiraffeAcademy) — Hugo tutorials covering the fundamentals. One of the few good video resources for getting started with Hugo.

---

## Tools I Use Daily

- **[Bitwarden](https://bitwarden.com)** — open source password manager. The single most impactful security tool most people are not using. Self-hostable if you want full control.
- **[Proton Mail](https://proton.me/mail)** — privacy respecting email with end to end encryption. My primary email provider via a custom domain.
- **[Proton VPN](https://protonvpn.com)** — open source VPN with a no logs policy. Part of the Proton ecosystem and audited independently.
- **[Proton Authenticator](https://proton.me/authenticator)** — open source 2FA app with end to end encrypted backup. Replaces Google Authenticator without the privacy trade-off.
- **[SimpleLogin](https://simplelogin.io)** — open source email alias service. Every account gets its own alias, compartmentalising exposure if a service is breached.
- **[NextDNS](https://nextdns.io)** — DNS based content filtering configured per device. Blocks trackers and malicious domains before a connection is made.
- **[Firefox](https://www.mozilla.org/firefox/)** — open source browser and my daily driver, hardened with uBlock Origin, Multi-Account Containers for session isolation, and telemetry disabled.
- **[Tor Browser](https://www.torproject.org)** — for genuine anonymity when it is needed, kept separate from daily browsing.
- **[Signal](https://signal.org)** — end to end encrypted messaging, open source, and the standard for secure communication.

---

{{< alert icon="envelope" cardColor="#173d24" iconColor="#4ade80" textColor="#e5e7eb" >}}
**Got a suggestion?**

This page reflects what I actually use today. If a resource has genuinely helped you on a similar path, get in touch at [contact@mail.fionnlinux.com](mailto:contact@mail.fionnlinux.com).
{{< /alert >}}
