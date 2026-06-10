# fionnlinux.com

Personal site and public learning journal documenting my path from electrical apprenticeship and manufacturing worker to cloud engineer — Linux foundations, security thinking, and infrastructure built in public.

Built with [Hugo](https://gohugo.io) and the [Blowfish](https://blowfish.page) theme. Deployed automatically via GitHub Actions to GitHub Pages with a custom domain via Cloudflare.

## Stack

- **Static site generator:** Hugo
- **Theme:** Blowfish
- **CI/CD:** GitHub Actions
- **Hosting:** GitHub Pages
- **Domain & CDN:** Cloudflare
- **Mirror:** Codeberg

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
content/
├── _index.md        # Homepage
├── about/           # About page
├── projects/        # Projects page
├── resources/       # Resources page
└── posts/           # Blog posts
```

## Deployment

Pushing to `main` triggers a GitHub Actions workflow that builds the site with Hugo and deploys to GitHub Pages automatically. The workflow handles Blowfish theme submodule initialisation before building. See `.github/workflows/` for the full pipeline configuration.

> **Note:** This repo is my personal site, not a template. The GitHub Actions workflow requires Pages to be enabled in your repo settings before deployment will work.

## Live Site

[fionnlinux.com](https://fionnlinux.com)

## Mirror

This repository is mirrored to [Codeberg](https://codeberg.org/fionnlinux/fionnlinux.com).
