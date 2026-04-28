---
title: "Building This Blog with Astro and Tailwind"
description: "Why I chose Astro for my developer blog, how the project is structured, and how the CI/CD pipeline works with GitHub Actions."
pubDate: 2026-04-28
tags: ["astro", "tailwind", "github-pages", "ci-cd"]
category: "Engineering"
coverImage: "https://images.unsplash.com/photo-1517694712202-14dd9538aa97?w=1200&h=600&fit=crop"
coverAlt: "Laptop with code on screen"
---

I wanted a personal blog that was fast, easy to write in, and cheap to host. Here's what I landed on and why.

## Why Astro

A few options I considered:

| Tool | Verdict |
|------|---------|
| Next.js | Overkill for a static blog |
| Jekyll | Dated; slow builds; Ruby dependency |
| Hugo | Great, but Go template syntax is cryptic |
| Docusaurus | Built for docs, not personal blogs |
| **Astro** | ✓ Built for content sites; markdown-first; zero JS by default |

Astro ships **zero JavaScript** to the browser unless you explicitly add interactive components. For a reading-focused blog, that's exactly what you want.

## Project structure

```
src/
  content/
    blog/            ← markdown posts go here
  components/
    PostCard.astro
    TagBadge.astro
    ReadingProgress.astro
    TableOfContents.astro
    ThemeToggle.astro
  layouts/
    BaseLayout.astro
    BlogPost.astro
  pages/
    index.astro
    blog/
      index.astro
      [...slug].astro
  styles/
    global.css       ← Tailwind v4 entry point
public/
  CNAME             ← custom domain for GitHub Pages
```

Each post is a markdown file with frontmatter:

```markdown
---
title: "My Post Title"
description: "A short summary."
pubDate: 2026-04-28
tags: ["astro", "tutorial"]
coverImage: "https://..."
---

Post content here...
```

Astro's **content collections** validate the frontmatter schema at build time — a missing `title` or wrong date format fails the build, not silently at runtime.

## Tailwind v4

Tailwind v4 dropped the `tailwind.config.js` entirely. Configuration now lives in CSS:

```css
@import "tailwindcss";
@plugin "@tailwindcss/typography";

/* Custom dark mode variant using data attribute */
@custom-variant dark (&:where([data-theme="dark"], [data-theme="dark"] *));

@theme {
  --font-sans: "Inter", system-ui, sans-serif;
  --font-mono: "JetBrains Mono", ui-monospace, monospace;
}
```

Much cleaner. No more JavaScript config files for a CSS framework.

## CI/CD with GitHub Actions

The workflow in `.github/workflows/deploy.yml` does three things on every push to `main`:

1. Install dependencies with `npm ci`
2. Build the site with `astro build` → outputs to `dist/`
3. Deploy `dist/` to GitHub Pages using the official `actions/deploy-pages` action

No extra secrets needed — GitHub's OIDC token handles authentication automatically.

## Custom domain

The `public/CNAME` file contains `blogs.huynhthientung.com`. GitHub Pages reads this and sets the custom domain automatically. On the Cloudflare side, a single CNAME record pointing `blogs` → `<username>.github.io` completes the setup.

## Reading experience features

A few things I added to make reading better:

- **Reading progress bar** — a 2px blue line at the top of the viewport
- **Table of contents** — auto-generated from headings, sticky sidebar on desktop, highlights current section via Intersection Observer
- **Dark mode** — default dark, toggle to light, preference saved in `localStorage`, anti-flash script in `<head>`

All implemented with vanilla JS and Astro's built-in `<script>` handling — no React, no extra runtime.

Simple, fast, and it works.
