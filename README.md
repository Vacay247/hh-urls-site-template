# hh-urls-site-template

**This is the template repository for all HH-URLS client websites.**

Do NOT edit individual client sites here. This repo is the "blueprint" that
gets forked every time HH-URLS onboards a new client. When you fix a bug or
add a feature, fix it HERE and it propagates to new clients automatically.

## Structure

```
.
├── index.html           # The full single-page mobile-first site
├── site.config.json     # Gets OVERWRITTEN per-client with their brand data
├── images/              # Gets OVERWRITTEN per-client with their photos
│   ├── hero.jpg
│   ├── gallery-1.jpg
│   ├── gallery-2.jpg
│   └── ...
├── netlify.toml         # Netlify build config (static site, no build step)
└── README.md
```

## How it works

1. HH-URLS clicks "New Client" → form submitted to Netlify Function
2. Orchestrator calls GitHub API's "Create repository using template" endpoint,
   which duplicates this repo into `youraccount/<client-slug>-site`
3. Orchestrator overwrites `site.config.json` with the client's real brand data
   (pulled from the `clients.brand_profile` column in Neon)
4. Orchestrator uploads client photos into `images/`
5. Netlify auto-deploys the new repo to `<slug>.netlify.app` and to
   `<slug>.easynow.life` via the custom domain
6. `index.html` loads `site.config.json` at page load and renders the right
   content, colors, hashtags, menu etc.

## Why config-driven instead of server-side templating

Netlify can serve this as a pure static site — no build, no functions, no
server render. It just loads the JSON on the client side and fills in the
page. Instant cold starts, minimum Netlify costs, maximum resilience.

The tradeoff: there's a ~50ms flash of unstyled/uncolored content while the
JSON loads. Mitigated by inlining a skeleton color scheme in the HTML.

## Editing the template

Clone this repo, edit `index.html`, commit, push. Done. New client onboardings
after that point will use the updated template. To backport a change to
EXISTING clients, use the admin UI's "Update site from template" button (TBD)
or manually cherry-pick the commit into the client repo.
