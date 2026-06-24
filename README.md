# skpqc.skworld.io

The landing + docs site for **sk_pqc** — hybrid post-quantum key encapsulation
for Dart & Flutter (**X25519 + ML-KEM-768**, FIPS 203, suite `x25519-mlkem768`).

Live: https://skpqc.skworld.io · Project repo: https://github.com/smilinTux/sk_pqc · Apache-2.0

## Layout

- `index.html` — single-page site (SKWorld design system, inline CSS + a tiny
  scroll-reveal script). Hero, "what it is" + honesty box, "why now"
  (harvest-now-decrypt-later), quick start, "how it works" (combiner formula,
  inline SVG diagram, wire-format table, web/native backends), "verified", and
  the ecosystem footer.
- `assets/og-card.{svg,png}` — 1200×630 social share card (referenced by the
  og:image / twitter:image meta tags). Regenerate the PNG with:
  `cairosvg assets/og-card.svg -o assets/og-card.png -W 1200 -H 630`.
- `CNAME` — `skpqc.skworld.io` (GitHub Pages custom domain).
- `robots.txt` / `sitemap.xml` / `llms.txt` — SEO + AI-crawler signals.
- `.nojekyll` — serve files as-is on GitHub Pages (no Jekyll processing).

## Content accuracy

All facts are pulled from the package `README.md` and `test_vectors/`. The site
deliberately avoids overclaiming: it is a **hybrid KEM** (secure if either leg
holds), **KEM-only** (signatures are future work), **FIPS 203 ML-KEM-768** tier
(not CNSA-2.0 ML-KEM-1024), and **not** "quantum-proof." The only original crypto
is the HKDF-SHA256 combiner; everything else binds audited libraries
(noble-post-quantum on web, liboqs on native).

## Deploy

Static site — GitHub Pages from the `main` branch root (`/`). The `CNAME` file
points it at `skpqc.skworld.io`; Cloudflare DNS (a `CNAME`/proxied record to the
GitHub Pages target) is configured separately. `.nojekyll` is present so the
`assets/` directory is served verbatim.

🔐 part of the SKWorld ecosystem · skworld.io · Apache-2.0
