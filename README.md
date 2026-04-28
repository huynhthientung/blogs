# blogs.huynhthientung.com

Personal developer blog built with [Astro](https://astro.build) and [Tailwind CSS](https://tailwindcss.com).  
Live at → **blogs.huynhthientung.com**

---

## Tech stack

| Tool | Role |
|---|---|
| [Astro 6](https://astro.build) | Static site framework |
| [Tailwind CSS v4](https://tailwindcss.com) | Styling |
| [@tailwindcss/typography](https://tailwindcss.com/docs/typography-plugin) | Prose styles |
| [Shiki](https://shiki.style) | Syntax highlighting |
| [GitHub Pages](https://pages.github.com) | Hosting |
| [GitHub Actions](https://github.com/features/actions) | CI/CD |

---

## Local development

```bash
# Install dependencies
npm install

# Start dev server at http://localhost:4321
npm run dev

# Build for production
npm run build

# Preview production build locally
npm run preview
```

---

## Writing a new blog post

### 1. Create a markdown file

Add a `.md` (or `.mdx`) file inside `src/content/blog/`:

```
src/content/blog/your-post-slug.md
```

The filename becomes the URL slug:  
`your-post-slug.md` → `blogs.huynhthientung.com/blog/your-post-slug`

---

### 2. Add frontmatter

Every post must start with a frontmatter block:

```markdown
---
title: "Your Post Title"
description: "A short summary shown in post cards and meta tags."
pubDate: 2026-05-01
tags: ["tag1", "tag2"]
category: "Engineering"
---

Your post content starts here...
```

**All frontmatter fields:**

| Field | Type | Required | Description |
|---|---|---|---|
| `title` | string | ✅ | Post title |
| `description` | string | ✅ | Short summary (shown in cards and `<meta>`) |
| `pubDate` | date `YYYY-MM-DD` | ✅ | Publication date |
| `tags` | string array | — | Tags (creates `/tags/[tag]` pages) |
| `category` | string | — | Category (creates `/categories/[category]` pages) |
| `updatedDate` | date `YYYY-MM-DD` | — | Shows "Updated" date on post |
| `coverImage` | string (URL) | — | Cover image shown at top of post and in cards |
| `coverAlt` | string | — | Alt text for cover image (accessibility) |
| `draft` | boolean | — | Set `true` to hide from all listings |

---

### 3. Write your content

Standard Markdown is fully supported:

````markdown
## Heading 2

### Heading 3

Normal paragraph with **bold**, _italic_, and `inline code`.

- Bullet list
- Another item

1. Ordered list
2. Another item

> Blockquote text here.

[Link text](https://example.com)

![Image alt text](https://example.com/image.jpg)

```javascript
// Fenced code block with syntax highlighting
const greet = (name) => `Hello, ${name}!`;
```
````

---

### 4. Use a cover image (optional)

Use any public image URL or place an image in `public/` and reference it by path:

```markdown
---
coverImage: "https://images.unsplash.com/photo-xxx?w=1200&h=600&fit=crop"
coverAlt: "Description of the image"
---
```

Or with a local image:

```markdown
---
coverImage: "/images/my-post-cover.jpg"
coverAlt: "Description of the image"
---
```

Place local images in `public/images/`.

---

### 5. Save as draft (optional)

Set `draft: true` to write without publishing. Draft posts are excluded from all listings and tag/category pages:

```markdown
---
title: "Work in Progress"
description: "..."
pubDate: 2026-05-01
draft: true
---
```

---

### 6. Publish

Push to `main` — GitHub Actions builds and deploys automatically:

```bash
git add src/content/blog/your-post-slug.md
git commit -m "add: new post — Your Post Title"
git push origin main
```

The deploy workflow runs in ~1 minute and the post goes live.

---

## Project structure

```
src/
  content/
    blog/               ← add your .md posts here
  components/
    PostCard.astro      — post card used in listings
    TagBadge.astro      — clickable tag pill
    TableOfContents.astro — auto-generated TOC (desktop sidebar)
    ReadingProgress.astro — scroll progress bar
    ThemeToggle.astro   — dark/light toggle
  layouts/
    BaseLayout.astro    — HTML shell, nav, footer
    BlogPost.astro      — post layout with TOC sidebar
  pages/
    index.astro         — homepage (recent posts)
    blog/index.astro    — all posts grouped by year
    blog/[...slug].astro — individual post
    tags/index.astro    — all tags
    tags/[tag].astro    — posts filtered by tag
    categories/index.astro    — all categories
    categories/[category].astro — posts filtered by category
  styles/
    global.css          — Tailwind v4 entry + dark theme
public/
  CNAME                 — custom domain (blogs.huynhthientung.com)
.github/
  workflows/
    deploy.yml          — CI/CD: build + deploy on push to main
```

---

## Custom domain

The `public/CNAME` file is already set to `blogs.huynhthientung.com`.

To activate it on Cloudflare:

1. Go to **Cloudflare → DNS** for `huynhthientung.com`
2. Add a CNAME record:
   - **Name:** `blogs`
   - **Target:** `<your-github-username>.github.io`
   - **Proxy:** DNS only (grey cloud)
3. In the GitHub repo → **Settings → Pages**, set the custom domain to `blogs.huynhthientung.com`

---

## CI/CD

| Event | Action |
|---|---|
| Push to `main` | Build → deploy |
| PR merged into `main` | Build → deploy |
| Manual trigger | Build → deploy (via Actions tab) |

Node.js 24 · `npm ci` · GitHub Pages artifact upload
