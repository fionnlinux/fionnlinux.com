---
title: "Projects"
date: 2026-05-19
draft: false
description: "Real repos built alongside my studies — this site, dotfiles, Python and Bash scripting, homelab services, and the projects still to come."
showDate: false
showReadingTime: false
showTableOfContents: true
showWordCount: false
showComments: false
showAuthor: false
showHero: true
heroStyle: "big"
imagePosition: "center"
---

{{< lead >}}
This page documents the projects I am actively building and learning from. Each one is a practical extension of my studies — real tools, real workflows, and real problems to solve. As my skills grow so will this page.
{{< /lead >}}

## fionnlinux.com

This site itself. Built with Hugo static site generator and the Blowfish theme, source mirrored on GitHub and Codeberg, deployed automatically via GitHub Actions to GitHub Pages.

**Stack:** Hugo, Blowfish, GitHub Actions, GitHub Pages

{{< cta url="https://github.com/fionnlinux/fionnlinux.com" label="GitHub" >}} {{< cta url="https://codeberg.org/fionnlinux/fionnlinux.com" label="Codeberg" style="outline" >}}

---

## dotfiles

My personal dotfiles, version controlled and publicly mirrored on GitHub and Codeberg. The goal is a reproducible environment, so a fresh Fedora or AlmaLinux machine can be brought close to my setup from a single clone. Documents how I work, how my tools are configured, and evolves alongside my Linux studies.

**Stack:** Vim, Bash, Git

{{< cta url="https://github.com/fionnlinux/dotfiles" label="GitHub" >}} {{< cta url="https://codeberg.org/fionnlinux/dotfiles" label="Codeberg" style="outline" >}}

---

## python-projects

A collection of Python projects written while learning to programme. Started small and building toward scripting, automation, and infrastructure tooling. First project written with my son.

**Stack:** Python 3

{{< cta url="https://github.com/fionnlinux/python-projects" label="GitHub" >}} {{< cta url="https://codeberg.org/fionnlinux/python-projects" label="Codeberg" style="outline" >}}

---

## bash-toolkit

A collection of Bash scripts for Linux administration and automation, built on Fedora Linux. Scripts are written to be readable, well-commented, and RHCSA-aligned. Actively growing alongside my Linux studies, and the scripts from the later parts of my planned Learning Bash series will land here.

**Stack:** Bash, Linux, GNU coreutils

{{< cta url="https://github.com/fionnlinux/bash-toolkit" label="GitHub" >}} {{< cta url="https://codeberg.org/fionnlinux/bash-toolkit" label="Codeberg" style="outline" >}}

---

## homelab-services

Self-hosted services running on my homelab, defined as rootless Podman Quadlets and managed natively by systemd. Currently runs Ollama with GPU passthrough for local LLM inference, and AdGuard Home for DNS filtering.

{{< alert "lightbulb" >}}
**Each service's README documents the real troubleshooting behind it:** rootless port binding under Podman's pasta networking, SELinux container labelling, and deliberate localhost-only scoping until there's a genuine reason to expose a service to the wider network.
{{< /alert >}}

**Stack:** Podman, Quadlet, systemd, SELinux

{{< cta url="https://github.com/fionnlinux/homelab-services" label="GitHub" >}} {{< cta url="https://codeberg.org/fionnlinux/homelab-services" label="Codeberg" style="outline" >}}

---

## Coming Soon

### uptime-kuma

A self-hosted monitoring dashboard on an AlmaLinux VM, run in Podman and managed by systemd. My first attempt, on Azure Container Apps, taught me why a monitoring tool needs a machine that stays on, so this is the second try.

### ansible-homelab

Configuration management for my homelab, starting with keeping my kids' two Zorin OS computers updated from my laptop with one command, and growing from there. It will be written up as a blog walkthrough.