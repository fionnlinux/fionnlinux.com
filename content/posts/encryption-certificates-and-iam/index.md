---
title: "Proving Who You Are — Encryption, Certificates, and the Many Ways to Log In"
date: 2026-07-13T00:00:00+01:00
draft: false
description: "Encryption at rest and in transit, PKI and self-signed certificates, the authentication and authorization protocols behind SSO and MFA, and geofencing — grounded in logging into work systems every day without ever thinking about what's actually happening underneath."
tags: ["networking", "security", "comptia"]
series: []
showTableOfContents: true
showReadingTime: true
showDate: true
showAuthor: true
---

I use single sign-on for work every day — one login gets me into several different systems without typing a password into each one separately. I also use multi-factor authentication on anything that actually matters, a password plus a code from my phone, and that one I did think about deliberately — sensitive accounts got it on purpose. The work login I gave far less thought to. It just worked, and I never stopped to ask what was actually carrying my identity between all those separate systems, or that the two things had their own distinct names and mechanisms underneath.

## Encryption — Protecting Data in Two Different States

Encryption gets talked about as one idea, but it actually has to solve two separate problems depending on where the data physically is.

**Data in transit** is data moving across a network right now — between your device and a server, for example. If it is not encrypted at this point, anyone in a position to observe that traffic can simply read it as it passes. This is exactly the problem HTTPS solves for web traffic — the connection between browser and website is encrypted while it travels, so anything sitting in between sees scrambled data rather than the actual content.

**Data at rest** is data sitting still — stored on a disk, a database, a backup. Encrypting data at rest means that even if someone gets physical access to the storage itself, or steals a backup, what they actually get is unreadable without the key. This one I already had covered without realising it fitted here: every Linux device in my house uses LUKS full-disk encryption, which is exactly this — if one of the laptops was stolen, the drive contents are unreadable without the passphrase. These are genuinely separate problems with separate solutions: data can be fully encrypted at rest and still be sent across the network in the clear, or the other way round, unless both are deliberately handled.

## Certificates — Proving a Server Is Who It Claims to Be

Encryption on its own answers "can anyone read this." Certificates answer a different question: "am I actually talking to who I think I am." A connection can be perfectly encrypted and still be with an impostor, if nothing verifies identity first.

**Public key infrastructure (PKI)** is the system that makes this verification work at scale. A trusted third party — a certificate authority — vouches for a certificate by signing it, and because your device already trusts that authority, it extends that trust to anything the authority has signed. This is why a certificate from a recognised authority causes no warning in a browser, while an unrecognised one does. It is also, I checked while writing this, exactly what is behind the padlock on fionnlinux.com — the certificate is issued by Let's Encrypt, a certificate authority browsers already trust, and I had never actually looked at who was vouching for my own site until now.

**Self-signed certificates** skip that trusted third party entirely — the certificate is signed by whoever created it, rather than by an authority anything else already trusts. That still gives you encryption, but not the same independent verification of identity, which is exactly why browsers warn about them. They are genuinely useful in a homelab or for internal testing, where you already know and trust the source yourself, but the wrong choice for anything public-facing where a stranger has no reason to trust your own signature.

## Identity and Access Management

**IAM** covers the two related but distinct questions of proving who you are, and then deciding what you are allowed to do once you have proven it. Those are **authentication** and **authorization**, and mixing them up is easy to do because they sit right next to each other in every login flow.

### Authentication — Proving Who You Are

**Multi-factor authentication (MFA)** means proving identity using two or more different categories of evidence, rather than just one. **Two-factor authentication (2FA)** — the code from my phone alongside my password — is simply MFA with exactly two factors. 2FA is not a separate thing from MFA, it is the specific, most common case of it.

**Single sign-on (SSO)** is what I use for work — authenticate once, and that single proof of identity is trusted across multiple separate systems, rather than logging into each one individually. It solves a different problem than MFA does. MFA is about how strong the initial proof is; SSO is about not having to repeat that proof over and over across every system that needs it.

Behind both of those sit protocols that actually carry the authentication information between systems:

- **RADIUS** — commonly used to centralise authentication for network access itself, such as logging onto Wi-Fi or a VPN.
- **LDAP** — a directory protocol, used to look up and verify user identities stored in a central directory.
- **SAML** — an XML-based standard specifically built to pass authentication between systems, which is what actually makes SSO work between separate applications.
- **TACACS+** — similar in purpose to RADIUS, more commonly associated with authenticating access to network devices themselves, like switches and routers.
- **Time-based authentication** — proving identity partly using a code that is only valid for a short rotating window, the mechanism behind the six-digit codes an authenticator app generates.

### Authorization — Deciding What You're Allowed to Do

Authorization only matters once authentication has already succeeded, and it answers a completely different question: now that the system knows who you are, what should you actually be allowed to touch.

**Least privilege** is the underlying principle — give an account only the access it genuinely needs to do its job, nothing broader. Without the formal name, this is already how the machines in my house are set up: my kids' accounts are standard users, and administrative rights through sudo stay with me. Their accounts can do everything they actually need and nothing more, which is the principle exactly, just applied at home rather than in an organisation. **Role-based access control** is the common way the same principle gets implemented at scale, assigning permissions to defined roles rather than to individual people one at a time, so access follows the job someone does rather than being configured from scratch for every new user.

## Geofencing

One more piece of logical security sits alongside IAM rather than inside it. **Geofencing** restricts access based on where a device physically is, using its location to decide whether a login should even be allowed to proceed — blocking a login attempt from a country an account has never operated in, for example, regardless of whether the password and MFA code entered were actually correct. It is a different kind of check entirely from anything above: not who you are or what you know, but where you are when you are asking.

## Why Separating These Actually Helps

Keeping encryption, certificates, and IAM as three genuinely separate ideas — rather than one blurred concept called "security" — makes it much easier to tell what has actually failed when something looks wrong. A padlock icon missing in a browser points at certificates, not encryption strength. A login that succeeds but reaches something it should not points at authorization, not authentication. And a service that is encrypted in transit but stored unprotected on disk is not actually secure just because one half of it is.

If any of this is new, there are two pieces of it you can put into practice today without spending anything. If you run Linux, LUKS full-disk encryption is offered during most distro installs — one checkbox, and data at rest on that machine is handled. And whatever you run, turning on two-factor authentication for your email and banking takes a few minutes per account and closes off the single most common way accounts get taken over. Both are the exact concepts above, applied for real.
