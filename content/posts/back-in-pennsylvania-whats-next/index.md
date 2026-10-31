---
title: "Back in Pennsylvania — What Happened, and What Comes Next"
date: 2026-10-30T00:00:00+01:00
draft: false
description: "An online exam that froze on question seven, six weeks in England, and the plan from here: Network+ by Christmas, then Security+, with Linux throughout."
tags: ["career", "comptia", "linux", "cloud"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
showComments: true
---

{{< lead >}}
It's been a while since my last post. In the meantime an online exam froze on question seven, we spent six weeks in England, and we came home to Pennsylvania. Here's what happened, and what's next for the site.
{{< /lead >}}

## The exam that froze on question seven

I sat the Network+ exam online through Pearson VUE. I was ready for hard questions. I wasn't ready for the software.

Every time I moved to the next question, the exam stopped and I had to message the proctor to get it going again. That happened on every single question up to question seven, where it froze completely and never came back. I had to close the software, and when I tried to get back in, it wouldn't let me, because the exam had already started.

I was furious. Online exams are stressful enough when everything works, and this one cost a lot of money. Sitting there watching it fall apart one question at a time was not how I pictured the day going.

Looking back, the one thing that went right was that I messaged the proctor every time it happened. It felt repetitive in the moment, but it meant there was a clear record showing the problem was their software, not my connection or my machine. I think that helped a lot with what happened next.

To be fair to Pearson VUE, they handled the aftermath well. They emailed the next day with an apology and a free retake voucher, which doesn't expire until next June. I'm aiming to sit it again **by Christmas**.

{{< alert "lightbulb" >}}
**If an online exam goes wrong,** tell the proctor straight away, every time, even when it feels like you're repeating yourself. If you end up needing a retake, that record is your evidence.
{{< /alert >}}

## Six weeks in England

The main reason things went quiet is that we spent the last six weeks in the UK. The original plan was to live in England for six months or more and see whether we wanted to move there for good.

After being there and travelling around, we decided to make it a long holiday instead, spending time with family and friends. We loved it. But we've lived in the US for ten years now, and it's where our kids have grown up. Trying it was the right call, because now we know: long term, Pennsylvania is the better fit for our family, and we want to make the most of it here. That includes buying our next house.

We'll be visiting England a lot more to see family and friends. And one day, if I can get to a remote role, I'd love to be able to travel more.

As for the quiet spell on the site: it turns out moving a family of five out of the country and then back into it again doesn't leave much time for blogging. Who knew.

## Back at work, and closer to IT

I've been fortunate to be hired back at the company I'd worked at for the last eight years, without losing any of that experience. They were really helpful once I let them know we were coming back, and I'm grateful for how smoothly it went.

The part I'm most excited about is the chance to **shadow the IT department**. So far that's meant seeing the everyday side of on-site IT support: imaging new computers over the network, getting the right software onto them, sorting out hardware, and working through support tickets. Studying for certifications teaches you the concepts, but watching a real IT team keep a busy workplace running is a different kind of learning.

It's also changed how I think about my next steps. My long-term goals haven't moved, but the most realistic way in is hands-on IT support experience, and that's what I'm focusing on building now. Shadowing teaches the practical side; my own study after work covers the theory behind it.

## My first Azure project, and a better second attempt

Just before we left, I built my first real cloud project in **Microsoft Azure**: hosting my own **Uptime Kuma** instance, an open source tool that checks whether websites and services are up and alerts you when they go down. I wanted to put what I'd been learning about cloud services into practice by building something I'd actually use. I kept an eye on it while we were away, but between the travelling I never had time to dig into it properly.

It ran on **Azure Container Apps**, Azure's service for running containers without managing any servers, with its database on **Azure Database for MySQL** and an **Azure Key Vault** for storing secrets. I'd set it to scale down to zero when idle, to keep the cost down. That suits a web app that only needs to run when someone visits, as long as the first visitor after a quiet spell doesn't mind a short wait. For a monitoring tool, it turned out to be the wrong choice, for two reasons:

1. **The monitoring stopped.** Uptime Kuma checks your sites from inside the app, on a timer, and nobody visits it to wake it up. Once it scaled down, the checks quietly stopped, and nothing told me, because the thing that would have told me had switched off. Keeping one copy always running would have fixed this one.
2. **Everything was wiped.** The app kept its data inside the container, and the container only had temporary storage. Every time it scaled to zero and started again, it came back blank and I had to create my admin account all over again. I'd started on an older version of Uptime Kuma, and switching to a newer image changed things, but it didn't fix the underlying problem: the data still wasn't landing anywhere permanent.

That's when I realised I was working against the tool. I'd built three Azure services around an app that really just wants one machine that stays on and keeps its files.

So the next version will run on a **RHEL-compatible VM running AlmaLinux**, with Uptime Kuma in a **Podman** container managed by the system like any other service. The VM stays on, so the checks never stop. It has its own disk, so the data survives restarts. And setting it up is practice for the RHCSA. AlmaLinux is binary compatible with Red Hat Enterprise Linux, so the commands are the same, and anyone following along can do it for free. That version will be worth documenting.

## What's next

### The certifications, in order

1. **Network+ retake, by Christmas.** It's booked in my head, if not yet on the calendar. All of my Network+ posts are in the [CompTIA Network+ series]({{< ref "/series/comptia-network+" >}}) if you're working through the same exam.
2. **Security+.** Security has run through everything on this site from the start, so it's the natural next step, and I'll write it up the same way I worked through Network+.
3. **MD-102, possibly.** Microsoft's certification for managing Windows devices, which is much closer to the day-to-day support work I've been seeing.
4. **AZ-900, then AZ-104.** The Azure certifications, which come after the ones that matter for getting established in IT support.
5. **RHCSA, later.** The Red Hat certification is still firmly on the list, and I'll dip into Red Hat administration along the way as standalone posts.

### Learning in public

Alongside the exams, there are a few things I'd like to share as I go:

- **Learning bash from the basics.** I don't write bash scripts often, so I'm planning a beginner series, built from scratch as I learn. Scripting is where Linux starts saving real time, and it's the first step towards automating things.
- **A+, from the top.** Mostly after Network+, I'm planning two series: Core 1, which covers hardware and networking, and Core 2, which covers operating systems, security and software. It'll keep the material fresh for the support side of IT, and hopefully help anyone studying for it, for free.
- **Homelab projects.** The Uptime Kuma rebuild on AlmaLinux, and managing my kids' computers with Ansible.
- **One-off Azure projects,** just for fun.

### Where this is heading

Linux is the part of IT I genuinely love. It's what I run every day and the heart of my homelab, and you can spend years trying to master it from the comfort of your own home. There's always something deeper to learn, and always another fun, exciting open source project to try or follow. The community around it is a big part of why I love it: people building things in the open and sharing them freely. That isn't changing, and I'll keep learning as much of it as I can.

At the same time, I'm realistic about breaking into IT. Windows and Microsoft products are the bread and butter of most workplaces, so that's the foundation I'm building my career on. Linux and Azure sit on top of that, and cloud engineering is still where I want to end up.

Thanks for sticking around while things were quiet. If you're on a similar path, or you've had your own online exam horror story, I'd like to hear about it in the comments.