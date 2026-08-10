# CLAUDE.md — Frontend Website Rules

> **Read `README.md` first.** It has the project structure, deployment setup
> (GitHub Pages via Actions, auto-deploys on push to `main`), and a "current
> state / where things stand" section describing what's built and what's
> still open (e.g. the contact form doesn't send anywhere yet). This file
> covers *how* to work on the project; the README covers *what it is* and
> *where it's at*.

## Always Do First
- **Invoke the `frontend-design` skill** before writing any frontend code, every session, no exceptions.

## Reference Images
- If a reference image is provided: match layout, spacing, typography, and color exactly. Swap in placeholder content (images via `https://placehold.co/`, generic copy). Do not improve or add to the design.
- If no reference image: design from scratch with high craft (see guardrails below).
- Screenshot your output, compare against reference, fix mismatches, re-screenshot. Do at least 2 comparison rounds. Stop only when no visible differences remain or user says so.

## Local Server
- **Always serve on localhost** — never screenshot a `file:///` URL.
- Start the dev server: `node serve.mjs` (serves the project root at `http://localhost:3000`, no dependencies — just Node's built-in `http`/`fs`)
- `serve.mjs` lives in the project root. Start it in the background before taking any screenshots.
- If the server is already running, do not start a second instance.

## Screenshot Workflow
- There is no `screenshot.mjs` / Puppeteer setup in this project — use the **Claude-in-Chrome browser tool** (navigate, screenshot, zoom) against `http://localhost:3000` instead.
- When taking a screenshot right after a scroll or an animation-triggering action, either wait ~1-2s or re-screenshot once — the first capture can land mid-transition (elements still fading/blurring in) and look broken when it isn't.
- `document.hidden` is `true` for automation tabs in this environment (they're not OS-focused), which throttles/pauses CSS and SVG SMIL animations and can make `scroll` events not fire on programmatic `window.scrollTo()`. Prefer the browser tool's real `scroll` action over JS-driven scrolling when you need animations/scroll listeners to actually run; verify animation logic via computed styles / `animVal` rather than assuming a static screenshot proves it's broken.
- When comparing to a reference, be specific: "heading is 32px but reference shows ~24px", "card gap is 16px but should be 24px"
- Check: spacing/padding, font size/weight/line-height, colors (exact hex), alignment, border-radius, shadows, image sizing

## Output Defaults
- Single `index.html` file, all styles inline, unless user says otherwise
- Tailwind CSS via CDN: `<script src="https://cdn.tailwindcss.com"></script>`
- Placeholder images: `https://placehold.co/WIDTHxHEIGHT`
- Mobile-first responsive

## Brand Assets
- Always check the `brand_assets/` folder before designing. It may contain logos, color guides, style guides, or images.
- If assets exist there, use them. Do not use placeholders where real assets are available.
- If a logo is present, use it. If a color palette is defined, use those exact values — do not invent brand colors.
- `brand_assets/blacklab-brand-v2.md` is the **current** positioning/voice/copy source of truth (AI-driven visualization studio for infrastructure & real estate). `brand_assets/Blacklab-Brand-Guidelines.html` is v1 — its visual identity (colors, logo, type) still carries over unchanged, but its business positioning (photography studio) is superseded. Don't reintroduce v1 copy/positioning.
- `brand_assets/brand_config.json` has the exact color/font tokens already wired into `index.html` as CSS custom properties — read from there, don't eyeball colors from screenshots.

## Anti-Generic Guardrails
- **Colors:** Never use default Tailwind palette (indigo-500, blue-600, etc.). Pick a custom brand color and derive from it.
- **Shadows:** Never use flat `shadow-md`. Use layered, color-tinted shadows with low opacity.
- **Typography:** Never use the same font for headings and body. Pair a display/serif with a clean sans. Apply tight tracking (`-0.03em`) on large headings, generous line-height (`1.7`) on body.
- **Gradients:** Layer multiple radial gradients. Add grain/texture via SVG noise filter for depth.
- **Animations:** Only animate `transform` and `opacity`. Never `transition-all`. Use spring-style easing.
- **Interactive states:** Every clickable element needs hover, focus-visible, and active states. No exceptions.
- **Images:** Add a gradient overlay (`bg-gradient-to-t from-black/60`) and a color treatment layer with `mix-blend-multiply`.
- **Spacing:** Use intentional, consistent spacing tokens — not random Tailwind steps.
- **Depth:** Surfaces should have a layering system (base → elevated → floating), not all sit at the same z-plane.

## Hard Rules
- Do not add sections, features, or content not in the reference
- Do not "improve" a reference design — match it
- Do not stop after one screenshot pass
- Do not use `transition-all`
- Do not use default Tailwind blue/indigo as primary color
