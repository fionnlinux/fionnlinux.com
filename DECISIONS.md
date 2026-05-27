# Site Decisions

A running log of significant decisions made building and maintaining fionnlinux.com — what was decided, why, and when. Written for future me and anyone curious about the reasoning behind the setup.

---

## Static site generator — Hugo
**Date:** May 2026  
**Decision:** Use Hugo over WordPress  
**Reasoning:** No database to compromise, no PHP attack surface, no plugins to maintain. Single binary install, fast build times, markdown content that lives in version control. Terminal first workflow fits daily Linux use. PageSpeed scores of 95 mobile and 98 desktop without any manual optimisation effort.

---

## Theme — Blowfish
**Date:** May 2026  
**Decision:** Use Blowfish theme over building from scratch or other themes  
**Reasoning:** Actively maintained by Nuno Coração and an active contributor community. Excellent documentation covering every configuration option. Terminal colour scheme available out of the box matching the site's visual identity. Built in shortcodes for alerts, lead paragraphs, badges and more. TypeIt animation support. Sensible defaults that produce a professional result immediately. Building a custom theme from scratch was considered but premature at this stage — revisit for the freetolearn.uk project when Hugo knowledge is deeper.

---

## Hosting — GitHub Pages with GitHub Actions CI/CD
**Date:** May 2026  
**Decision:** Deploy to GitHub Pages via GitHub Actions on every push to main  
**Reasoning:** Free tier sufficient for a personal site. CI/CD is a core skill appearing consistently in Linux and DevOps job postings — learning it through real use rather than a tutorial is more effective. Every push to main triggers an automatic build and deploy with no manual steps. Netlify was considered and remains an option but GitHub Pages met all requirements without introducing a separate platform dependency. Netlify is reserved for client projects like the planned juniatalaw.com migration where its form handling and build environment are more appropriate.

---

## Mirror — Codeberg dual push
**Date:** May 2026  
**Decision:** Mirror all repositories to Codeberg via dual push remote configuration  
**Reasoning:** Redundancy against GitHub outages, policy changes, or account issues. Philosophical alignment with open source values and avoiding single platform dependency. Codeberg is a non-profit, community run Git forge that aligns with the site's open source values. Dual push means one git push command keeps both in sync automatically with no additional overhead or workflow change.

---

## Domain registrar and CDN — Cloudflare
**Date:** May 2026  
**Decision:** Register fionnlinux.com through Cloudflare Registrar, use Cloudflare CDN  
**Reasoning:** Cloudflare handles both domain registration and DNS in one place, reducing attack surface compared to a separate registrar and CDN combination. At-cost domain pricing with no markup. Free CDN tier provides DDoS protection, caching, and SSL termination. Auto-renewal enabled to prevent accidental expiry. Account secured with 2FA.

---

## Contact email — Proton Mail with SimpleLogin alias
**Date:** May 2026  
**Decision:** Use contact@mail.fionnlinux.com as the public facing contact address for the site  
**Reasoning:** Own the domain, own the identity infrastructure. The alias is hosted on a custom subdomain via SimpleLogin — included in the Proton Unlimited subscription — and forwards to a Proton Mail inbox. If Proton ever changes direction, updating DNS MX records migrates the entire email infrastructure to a new provider without the contact address changing. Domain portability was the key requirement. Using an alias rather than a direct inbox means the real email address is never exposed publicly, reducing spam and phishing surface.

---

## Analytics — Umami
**Date:** May 2026  
**Decision:** Use Umami over Google Analytics, Plausible, or no analytics  
**Reasoning:** Cookie-free by design — no consent banner required, fully compliant with UK GDPR and PECR without additional configuration. Collects no personal data — only anonymous aggregate statistics. Open source under MIT licence — self-hostable on Hetzner as the homelab develops if the cloud tier ever becomes insufficient. Free cloud tier supports three websites and 100,000 monthly events — more than sufficient at this stage. Google Analytics was considered and rejected because its tracking model conflicts with the site's privacy values and would require a cookie consent banner. Plausible was considered but has no free tier.

---

## Content licence — no explicit licence
**Date:** May 2026  
**Decision:** No licence file, no licence section in README  
**Reasoning:** Site content is personal writing — default copyright applies. The repository is public for transparency and community learning, not for redistribution. Hugo configuration files and theme settings are not complex enough to warrant a separate licence at this stage. If any configuration or tooling becomes genuinely reusable by others a proper licence file will be added at that point.

---

## Taxonomy — tags and series only, no categories
**Date:** May 2026  
**Decision:** Use tags and series taxonomies, disable categories  
**Reasoning:** Categories add an organisational layer that is premature with a small number of posts. Tags provide flexible content organisation at the current scale. Series will link related posts when walkthroughs begin referencing earlier concept posts. Revisit if post volume grows to a point where categories would genuinely help navigation.

---

## Feature images — SVG for concepts, screenshots for walkthroughs
**Date:** May 2026  
**Decision:** Use animated featured.svg for concept and narrative posts, clean screenshots cropped to 1200x630 in GIMP for walkthrough posts  
**Reasoning:** Visual distinction between post types before the reader clicks. SVG files are a few KB versus 100-200KB for JPEG — multiple concept posts sharing the same SVG improves page speed. Screenshots signal hands on technical content and set accurate reader expectations. No stock photos, no AI generated images. SVGs are browser cached after first load so the animated featured.svg costs nothing on subsequent page views.

---

## Comments — Giscus, deferred until post 10
**Date:** May 2026  
**Decision:** Plan to add Giscus comments after post 10, no comments before that point  
**Reasoning:** Comments on a site with few posts creates maintenance overhead with minimal engagement benefit. GitHub Discussions via Giscus is the right fit for a technical audience — most readers in the Linux and security community already have GitHub accounts. Waiting until post 10 ensures there is enough content for meaningful discussion before the feature is added. Contact via email alias is available for anyone who needs to reach out in the meantime.

---

## Version control — conventional commits
**Date:** May 2026  
**Decision:** Use conventional commit format throughout — feat:, fix:, docs:, chore:, ci:  
**Reasoning:** Professional habit worth building early. Clear commit history is useful when reviewing changes, debugging deployment issues, and demonstrating good practice to anyone reviewing the repository. Consistent from the start is easier than retrofitting the habit later.
