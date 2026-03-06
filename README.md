# Abbika Website

Corporate website for [Abbika LLC](https://abbika.com) — an AI-powered mobile apps and Web3 innovation startup.

## Tech Stack

- **Framework:** [Astro](https://astro.build/) (static site generation)
- **Hosting:** Cloudflare Pages
- **SEO:** Auto-generated sitemap, structured data (JSON-LD), Open Graph, Twitter Cards

## Project Structure

```
├── src/
│   ├── layouts/
│   │   └── Layout.astro      # Base layout with SEO head, styles, and structured data
│   └── pages/
│       └── index.astro        # Homepage content
├── public/
│   ├── css/                   # Bootstrap grid, icon fonts
│   ├── one-page/              # Logo, et-line icon font
│   ├── robots.txt             # Crawl directives + sitemap reference
│   ├── _redirects             # Cloudflare: www → non-www 301 redirect
│   └── _headers               # Cloudflare: security headers + cache control
├── astro.config.mjs           # Astro config (site URL, sitemap integration)
├── package.json
└── tsconfig.json
```

## Development

```bash
npm install
npm run dev       # Start dev server at localhost:4321
npm run build     # Build static site to dist/
npm run preview   # Preview built site locally
```

## Deploying to Cloudflare Pages

### Initial Setup

1. Go to [Cloudflare Dashboard](https://dash.cloudflare.com/) > **Workers & Pages** > **Create application** > **Pages**
2. Connect your GitHub repository
3. Configure build settings:
   - **Build command:** `npm run build`
   - **Build output directory:** `dist`
   - **Environment variable:** `NODE_VERSION` = `18`

### Custom Domain (abbika.com)

1. In Cloudflare Pages project > **Custom domains**, add:
   - `abbika.com`
   - `www.abbika.com`
2. In **Cloudflare DNS**, ensure:
   - `A` or `CNAME` record for `abbika.com` pointing to the Pages project
   - `CNAME` record for `www` pointing to `abbika.com`
3. The `_redirects` file handles `www.abbika.com → abbika.com` with a 301 permanent redirect

**Why non-www as canonical?** Using the bare domain (`abbika.com`) as the canonical URL avoids duplicate content issues. All `www.` requests are 301-redirected, consolidating link equity and search signals to a single URL.

### Alternative: Cloudflare Redirect Rules (Edge-Level)

For more reliable www-to-non-www redirects (handled at the CDN edge before hitting your site), you can also create a redirect rule in **Cloudflare Dashboard > Rules > Redirect Rules**:

- **When:** Hostname equals `www.abbika.com`
- **Then:** Dynamic redirect to `concat("https://abbika.com", http.request.uri.path)`
- **Status code:** 301

This runs at the edge and is faster than `_redirects`, though both approaches work.

## SEO Features

### Meta Tags
- Primary meta: title, description, robots, googlebot directives
- Canonical URL on every page
- Open Graph: type, url, title, description, image, site_name, locale
- Twitter Cards: summary_large_image

### Structured Data (JSON-LD)
- **Organization** — company info, contact points, owned products (VerbPal, TaskJogger)
- **WebSite** — site-level schema
- **WebPage** — per-page schema

### Technical SEO
- Auto-generated `sitemap-index.xml` via `@astrojs/sitemap`
- `robots.txt` with sitemap reference
- Semantic HTML5 (`<main>`, `<article>`, `<nav>`, `<section>`, `<address>`)
- Proper heading hierarchy (single `<h1>`, structured `<h2>`–`<h5>`)
- `aria-label` on sections, `aria-hidden` on decorative icons
- Font preconnect hints for Google Fonts
- Native `scroll-behavior: smooth` (no jQuery)
- Responsive dark mode via `prefers-color-scheme`

### Security Headers (via `_headers`)
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Content-Security-Policy` (self + Google Fonts)
- `Permissions-Policy` (camera, mic, geo disabled)
- Long-lived cache headers on static assets

## Cross-Linking Strategy

The site links to Abbika's product sites for SEO backlinking:

| Product | URL | Context |
|---------|-----|---------|
| **VerbPal** | https://www.verbpal.com | AI language learning for children |
| **TaskJogger** | https://www.taskjogger.com | Intelligent task management |

Links appear in: hero subtitle, mission bridge, mission cards, about section, product cards (with CTAs), vision phase 1, and footer product nav.

**For maximum SEO benefit**, each product site should link back to `https://abbika.com` (e.g., "An Abbika product" in their footers). This creates a reciprocal link network that strengthens domain authority for all three domains.

## Adding New Pages

Create a new `.astro` file in `src/pages/`:

```astro
---
import Layout from '../layouts/Layout.astro';
---

<Layout
  title="Page Title — Abbika"
  description="Page description for search engines."
>
  <!-- page content -->
</Layout>
```

The sitemap updates automatically on build.
