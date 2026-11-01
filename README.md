# fionnlinux.com

My study-notes site and public learning journal: CompTIA certification notes, Linux, IT support, cloud and homelab projects, written as I learn. I'm working my way from manufacturing into IT, with Linux as the passion and cloud engineering as the long-term goal.

Built with [Hugo](https://gohugo.io) and the [Blowfish](https://blowfish.page) theme. Deployed automatically via GitHub Actions to GitHub Pages with a custom domain via Cloudflare.

## Stack

- **Static site generator:** Hugo (extended)
- **Theme:** Blowfish, as a git submodule
- **CI/CD:** GitHub Actions
- **Hosting:** GitHub Pages
- **Domain & CDN:** Cloudflare
- **Analytics:** Umami (cookie-free)
- **Comments:** Giscus, backed by GitHub Discussions
- **Fonts:** Press Start 2P and Roboto Mono (Roboto Mono is self-hosted)
- **Mirror:** Codeberg

## Versions

- **Hugo:** 0.162.1 extended, pinned in `.github/workflows/hugo.yml` so the live build matches my laptop
- **Blowfish:** v3.6.0, pinned as a submodule

Each Blowfish release supports a specific range of Hugo versions, and newer releases need a newer Hugo than 0.162.1. Before upgrading either one, check the supported range in Blowfish's release notes.

## Running Locally

Clone the repo with submodules:

```bash
git clone --recurse-submodules https://github.com/fionnlinux/fionnlinux.com
cd fionnlinux.com
```

The `--recurse-submodules` flag pulls in the Blowfish theme, which is included as a git submodule rather than copied into this repo.

Start the local development server:

```bash
hugo server -D
```

Visit `http://localhost:1313` in your browser.

## Structure

```
.github/workflows/   # Build and deploy pipeline
assets/              # Custom CSS, pixel icons and images
config/_default/     # Hugo and theme configuration
content/
├── _index.md        # Homepage
├── about/           # About page
├── posts/           # Blog posts
├── projects/        # Projects page
├── resources/       # Resources page
└── series/          # Series pages
layouts/             # Template overrides
static/              # Favicons and fonts
themes/blowfish/     # Blowfish theme (git submodule)
```

## Deployment

Pushing to `main` triggers a GitHub Actions workflow that builds the site with Hugo and deploys to GitHub Pages automatically. The workflow handles Blowfish theme submodule initialisation before building. See `.github/workflows/` for the full pipeline configuration.

> **Note:** This repo is my personal site, not a template. The GitHub Actions workflow requires Pages to be enabled in your repo settings before deployment will work.

## Live Site

[fionnlinux.com](https://fionnlinux.com)

## Mirror

This repository is mirrored to [Codeberg](https://codeberg.org/fionnlinux/fionnlinux.com).