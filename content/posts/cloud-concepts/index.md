---
title: "The Cloud Is Just Someone Else's Computer — Making Sense of Cloud Concepts"
date: 2026-06-05T00:00:00+01:00
draft: false
description: "Cloud was a buzzword I heard everywhere and understood nowhere. Here is what finally made it click, the service and deployment models explained through things I actually use, and the hosting decision that taught me what multitenancy really means."
tags: ["networking", "security", "cloud", "comptia", "soc"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

For a long time the cloud was a word I heard constantly and understood barely at all. Every tech podcast I listened to talked about it as though it were obvious, and I nodded along without ever being able to say what it actually was. It sounded like something vast and abstract floating above everything.

The answer turned out to be simpler than expected. The cloud is just someone else's computer. Instead of running a server on-site, in your own building, you rent space on a machine that lives in someone else's datacentre and reach it over the internet. That is the whole idea at its core. Everything else — the service models, the deployment models, the terminology — is detail built on top of that one plain fact. Once that settled, the rest of the topic started to make sense.

This post walks through the cloud concepts from the Network+ objectives using real examples.

## The Service Models — Who Manages What

The three service models are really just answers to one question: how much of the work do you do, and how much does the provider do? They run from the provider managing almost everything to you managing almost everything.

**Software as a Service (SaaS).** The provider runs everything and you just use the software. You do not touch the server, the operating system, or the infrastructure underneath. You open an app and use it.

Proton Mail is a good example from my own setup. I use it every day, but I do not run a mail server, patch it, or keep it online — Proton does all of that. I just use the service. Most of the privacy-focused tools I rely on are SaaS in the same way: I use the software, someone else runs the machinery.

**Platform as a Service (PaaS).** The provider manages the infrastructure and the platform, and you manage what runs on top of it. You do not configure the server or keep the operating system patched, but you are responsible for your own application or content.

This site, and the law firm site I run for a relative, are both good examples. The law firm site runs on Rocket.net, a managed WordPress hosting platform. Everything underneath — the server, the security patching, the performance tuning — is handled for me. I just manage the website itself. This very site is similar: it runs on GitHub Pages, which builds and serves it for me. I push the content and GitHub handles the rest. I never touch a server.

**Infrastructure as a Service (IaaS).** The provider gives you the raw infrastructure — a virtual server — and everything above that is your responsibility. You choose the operating system, configure it, secure it, patch it, and keep it running.

The contrast with PaaS is what made this distinction land for me. If instead of GitHub Pages I had rented a virtual private server and set up a web server like Nginx on it myself, that would be IaaS. I would be responsible for configuring it properly, locking it down, and keeping it updated. The provider gives you the machine; everything else is on you. PaaS hands you a managed platform; IaaS hands you a bare server and a lot more responsibility.

The simplest way I hold the three together: with SaaS you manage nothing but your use of the software, with PaaS you manage your application, and with IaaS you manage everything except the physical hardware.

## A Note on Trust

Studying the service models changed how I think about them, not just how I define them. Every time you use a cloud service, you are handing part of your setup to someone else to run — and with it, often, your data.

That has made me more deliberate about who I choose. I lean towards open source and privacy-focused providers like Proton, and I try to favour services that have a clear exit strategy, so I am not cornered into a product I cannot leave. The convenience of letting someone else run the infrastructure is real, but it comes with a dependency, and being thoughtful about that dependency is part of using the cloud sensibly rather than just conveniently.

## The Deployment Models — Whose Cloud Is It

Where the service models are about who manages what, the deployment models are about where the cloud lives and who it belongs to.

**Public cloud.** Infrastructure owned and run by a provider like Amazon Web Services, Microsoft Azure, or Google Cloud, rented out to anyone. You are using their hardware in their datacentres. This is what most people mean by "the cloud."

**Private cloud.** Cloud infrastructure dedicated to a single organisation, often run in-house or hosted privately. It gives more control and isolation, but it is more expensive because you are not sharing the cost across many tenants.

**Hybrid cloud.** A mix of both — some workloads in the public cloud, some kept private, connected together. Organisations use this to keep sensitive systems private while taking advantage of the public cloud's scale for everything else.

## The Cloud Networking Pieces

A few networking concepts come up specifically in the cloud context. These were the parts I found hardest to picture at first, because they are virtual versions of things that used to be physical.

**Virtual Private Cloud (VPC).** A logically isolated section of a public cloud that you control. Even though you are renting space on shared provider infrastructure, your resources sit inside your own private virtual network. You define the IP ranges and subnets and decide what can reach what. It is your own private network, hosted inside someone else's datacentre.

**Network security groups and security lists.** Network security groups are essentially virtual firewall rules applied to cloud resources, controlling what traffic can reach a virtual machine. Security lists do a similar job but apply at the subnet level rather than to individual resources — a set of rules covering everything in a subnet rather than a single instance. Both are the same firewall idea from a physical network, applied in the cloud at different scopes.

**Cloud gateways.** These connect your cloud network to other networks. An internet gateway allows resources in your cloud network to reach the internet. A NAT gateway lets resources in a private subnet reach out to the internet without each having a public IP of their own — the same translation idea a home router performs with NAT, applied in the cloud.

**Network Functions Virtualisation (NFV).** The idea of running network functions that once required dedicated hardware — firewalls, routers, load balancers — as software on general-purpose hardware instead. This one I understand through a project of my own. Rather than buying a proprietary firewall appliance, my plan is to run firewall software on a general-purpose device. That is the same principle as NFV: the function matters, not the box it runs on.

**Cloud connectivity.** There are two main ways to connect your own premises to a cloud provider. One is a VPN over the public internet, which is simpler but depends on the internet's reliability. The other is a dedicated private connection — AWS calls it Direct Connect — which bypasses the public internet entirely for faster, more consistent, more secure connectivity. Enterprises use the dedicated option when reliability and security matter enough to justify the cost.

## The Characteristics — Why the Cloud Is Attractive

Three characteristics come up in the objectives, and they explain why organisations move to the cloud in the first place.

**Scalability.** The ability to increase capacity as demand grows. If a business expands and needs more resources, the cloud lets it add them without buying and installing physical hardware.

**Elasticity.** Related but distinct. Elasticity is the ability to scale up to meet a temporary spike in demand and then scale back down when it passes, rather than paying for peak capacity all the time. A retailer might scale up for a busy sale period and back down afterwards.

**Multitenancy.** Multiple customers sharing the same underlying infrastructure while being kept logically isolated from one another. It is what makes the public cloud cost-effective — the provider spreads the cost of the hardware across many tenants.

Multitenancy is the one I understand through a real decision. When I was choosing hosting for the law firm site, shared hosting was the cheaper option — many sites sharing one server. I deliberately chose dedicated, isolated hosting instead, even though it cost more. My reasoning was security: a shared server means a larger attack surface, where a compromise of another site on the same machine could potentially put mine at risk. For a law firm handling sensitive information, the isolation was worth paying for. That decision taught me what multitenancy actually means in practice, and when its trade-offs are worth avoiding.

## Seeing It in What You Already Use

The cloud stopped being a vague buzzword once I stripped it back to its simplest truth — it is someone else's computer, rented over the internet. Everything after that is just degrees of who manages what and where the machine lives, and once you have that frame the rest falls into place far more easily than it first appears.

If you are studying this, a good way to make it concrete is to look at the services you already rely on through this lens. The email you use, the place your website is hosted, the storage service syncing your files — most of them are cloud services, and working out which model each one fits is a genuinely useful exercise. Is your email SaaS? Is your hosting PaaS or IaaS? Is it on a shared server or an isolated one? Use this post as a reference and categorise a few of your own tools. It is a quick thing to do and it turns the concepts from words on a page into something you can see all around you.
