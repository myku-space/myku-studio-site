# myku.studio

The landing page for **myku.studio** — a single-screen "threshold" that presents
the wordmark and a haiku, and offers one quiet path onward to
[myku.space](https://myku.space). See [`DESIGN.md`](./DESIGN.md) for the full
design spec.

A static page served by a Cloudflare **Worker with static assets** (no server
code — Cloudflare serves `public/` from its edge).

## Structure

```
public/index.html   the page (all CSS inline)
DESIGN.md           design spec / token reference
wrangler.jsonc      Cloudflare Worker config (assets-only)
package.json        dev/deploy scripts
```

## Local development

```bash
npm install        # first time only — installs wrangler
npm run dev        # serves at http://localhost:8787 with live reload
```

`npm run dev` runs `wrangler dev`, which serves `public/` locally exactly as
Cloudflare will in production. Edit `public/index.html` and refresh.

For a zero-dependency quick look you can also just open `public/index.html`
directly in a browser — there's no build step.

## Deploying

Deploys are **published from GitHub** via Cloudflare Workers Builds. One-time setup:

1. Push this repo to GitHub.
2. In the Cloudflare dashboard: **Workers & Pages → Create → Workers →
   Import a repository**, and select this repo.
3. Build settings:
   - **Build command**: *(leave empty — nothing to build)*
   - **Deploy command**: `npx wrangler deploy`
4. Save. Every push to `main` now builds and deploys automatically.

To deploy manually from your machine instead:

```bash
npx wrangler login     # first time only
npm run deploy         # wrangler deploy
npm run check          # dry-run validation, no deploy
```

## The App Store link

The footer's `| APP STORE` link is hidden until the app ships. To reveal it,
add the class `appstore-live` to `<body>` in `public/index.html` and set the
real store URL on the `App Store` anchor's `href` (currently `#`).
