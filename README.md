# Black Lab Visuals — Website

Marketing site for **Black Lab Visuals**, an AI-driven visualization studio for
construction, infrastructure, and real estate (cinematic 3D renders, retrofit
before/after sequences, and interactive web experiences).

**Live site:** https://cesardemacedo.github.io/blacklab-visuals-website/
**Repo:** https://github.com/CesarDeMacedo/blacklab-visuals-website

## Tech stack

No build step, no framework, no npm dependencies.

- Single `index.html` — all markup, all CSS (in a `<style>` block), all JS (in a
  `<script>` block at the bottom).
- [Tailwind CSS via CDN](https://cdn.tailwindcss.com) for utility classes, plus
  hand-written CSS for anything Tailwind can't express (the aperture-shutter
  hover effect, the line-reveal text animation, the gradient-tracing ring).
- Fonts loaded from Google Fonts: Montserrat (headings), Cormorant Garamond
  (italic accent), Inter (body).
- Vanilla JS: one `IntersectionObserver` drives all scroll-reveal animations.
  No animation library.

## Project structure

```
index.html                  the entire site
serve.mjs                   tiny static file server for local dev (no deps)
brand_assets/
  brand_config.json         canonical brand tokens: colors, fonts, logo path
  blacklab-brand-v2.md       brand guide v2 — positioning, voice, copy rules, structure
  Blacklab-Brand-Guidelines.html   full v1 visual identity guidelines (large file, has embedded images)
  blacklabvisuals_logo_01.png      logo (labrador profile + 6-blade aperture), transparent PNG
  bg_tile_inner.png / bg_tile_outer.png   subtle dark texture tiles (unused currently)
portfolio/
  *.jpg                     real portfolio renders (see "Portfolio images" below)
.github/workflows/
  deploy-pages.yml          auto-deploys to GitHub Pages on every push to main
```

## Local development

```bash
node serve.mjs
```

Serves the project root at `http://localhost:3000`. No install step — `serve.mjs`
only uses Node's built-in `http`/`fs` modules. Open `http://localhost:3000` in a
browser; there's no live-reload, just refresh after editing `index.html`.

There's no `screenshot.mjs` / Puppeteer setup in this project (an earlier
version of `CLAUDE.md` referenced one from a different machine/template — it
doesn't apply here). For visual QA, use a real browser — this project has been
built and screenshotted via the Claude-in-Chrome browser tool.

## Deployment

Push to `main` → GitHub Actions (`.github/workflows/deploy-pages.yml`) builds
and deploys automatically to GitHub Pages. Nothing to run manually. The repo is
public (required for free GitHub Pages on a free account); there are no
secrets or credentials anywhere in this project, so that's safe.

To check a deploy: `gh run list --repo CesarDeMacedo/blacklab-visuals-website`

## Brand system

Source of truth: `brand_assets/brand_config.json` (colors/fonts) and
`brand_assets/blacklab-brand-v2.md` (positioning, voice, copy rules, required
page structure). Read `blacklab-brand-v2.md` before making content changes —
it defines what the studio does, the tone (quiet confidence, no hype), and
words to avoid ("disruptive", "cutting-edge", "AI-powered" as a headline).

Key tokens (also defined as CSS custom properties in `index.html`):

| Token | Value |
|---|---|
| `--gold` | `#C5A572` |
| `--gold-light` | `#E8C87A` |
| `--gold-dark` | `#8C7040` |
| `--bg` | `#050505` |
| `--bg-alt` | `#101010` |
| `--bg-soft` | `#1A1A1A` |
| `--cream` | `#F5F2EE` |
| Headings | Montserrat, bold/extrabold, tight tracking |
| Accent | Cormorant Garamond, italic |
| Body | Inter |

## Portfolio images

`portfolio/*.jpg` are real renders selected and compressed from the client's
portfolio archive (`C:\Users\cesar\Downloads\A_VIDEOS\Portfolio` — not part of
this repo, lives only on the local machine). Originals were 5–30MB PNGs;
everything here was resized to max 1800px wide and re-encoded as JPEG q82
(~200–530KB each) for web performance. If you need to swap or add images,
pull fresh source files from that folder and repeat the resize/compress step
rather than committing multi-MB PNGs.

Currently used on the site (in `#work`):

- `luxury-exterior.jpg` — Luxury Waterfront Residence, aerial
- `retrofit-after.jpg` — Facade retrofit (honeycomb), after
- `civic-hero.jpg` — Civic/public building, blue hour
- `digital-twin.jpg` — Building intelligence cutaway (digital twin data overlay)
- `luxury-interior.jpg` — Waterfront residence living room

Not currently used but available: `luxury-amenity.jpg`, `retrofit-before.jpg`,
`retrofit-construction.jpg` (the full before → construction → after triptych
for the retrofit project), `infra-future-vision.jpg`.

## Current state / where things stand

The site went through several iterations this project — see git log for the
full history. As of the latest commit:

- **Positioning**: v2 brand (AI-driven visualization studio for infrastructure
  & real estate) — this replaced an earlier v1 photography/wedding-studio
  concept. If you see copy that sounds like a photography studio, it's stale.
- **Sections** (in order): Hero → Aperture Principle manifesto → Work
  (portfolio grid) → What We Do (core services + delivery formats) → Process
  → About (the mark / brand story) → Contact → Footer.
- **Work section** is a static grid (1 featured tile + 4 regular), each with
  an aperture-shutter hover reveal. This was deliberately reverted from a
  scroll-pinned gallery version — the client preferred the simpler static
  grid. Don't reintroduce scroll-pinning without being asked.
- **Animations**: line-by-line blur reveal on the hero H1 and the manifesto
  quote; scroll-triggered fade-ups elsewhere via `IntersectionObserver`;
  animated gold gradient tracing a ring behind the logo in the About section
  (pure SVG/SMIL, no JS animation library); nav fade-in + arrow-sweep on
  hover. Everything respects `prefers-reduced-motion`.
- **Known limitation**: the contact form (`#contact`) has
  `onsubmit="event.preventDefault()"` — it doesn't send anywhere yet. Before
  this goes fully live, wire it to a form backend (Formspree, Netlify Forms,
  a serverless function, etc.) or connect `hello@blacklabvisuals.co` properly.
- **Deployed**: yes, live on GitHub Pages, auto-deploys on push (see above).

## Style rules for future changes

See `CLAUDE.md` for the full design/process ruleset this project follows
(anti-generic guardrails, brand asset usage, screenshot-driven QA workflow).
