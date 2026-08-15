# skpqc.skworld.io

The landing + docs site for **sk_pqc**, a hybrid post-quantum key-encapsulation
suite (**X25519 + ML-KEM-768**, FIPS 203, suite id `x25519-mlkem768`) shipped as
three implementations that share one wire format.

This repository holds the **website only**. It contains no cryptographic code.

- Custom domain (from `CNAME`): `skpqc.skworld.io`
- Upstream implementations, all Apache-2.0:
  - Python: <https://github.com/smilinTux/sk-pqc-py> (`pip install sk-pqc`)
  - Dart / Flutter: <https://github.com/smilinTux/sk-pqc-dart> (`dart pub add sk_pqc`)
  - Rust: <https://github.com/smilinTux/sk-pqc-rs> (`cargo add sk-pqc`)

> The old `github.com/smilinTux/sk_pqc` URL now only resolves through a GitHub
> rename redirect to `sk-pqc-dart`. Link the real repo names above.

## Honest-claims posture

The published page carries this posture in four places (the honesty banner, the
`description` / `og:description` / `twitter:description` meta tags, the "Honest
boundary" section, and the footer). It is repeated here so the repository states
the same thing as the artifact it publishes.

- **Experimental and unaudited.** sk_pqc has had no third-party security audit,
  fuzzing, or formal review. It is research-grade sovereign software.
- **Post-quantum, never "quantum-proof."** The defensible word is
  *post-quantum* / *quantum-resistant*. The words "quantum-proof",
  "quantum-safe", and "unbreakable" are rejected mechanically by the library's
  `report` module and must not appear as a claim on this site.
- **Hybrid means secure if EITHER leg holds.** The derived secret survives as
  long as either the classical X25519 leg or the ML-KEM-768 leg is unbroken.
  Never both required.
- **KEM only.** No signatures yet. ML-KEM-768 is standardized as **FIPS 203**;
  the companion signature standard **FIPS 204** (ML-DSA) is referenced, not
  implemented.
- **-768 tier, not -1024.** The site does not claim the CNSA-2.0 ML-KEM-1024
  tier.
- **AES-256 is not quantum-broken.** Symmetric primitives are Grover-only. The
  site must never imply otherwise.
- The only original crypto is the HKDF-SHA256 combiner. Everything else binds
  vetted libraries (liboqs and `@noble/post-quantum` and pyca `cryptography`).

Every factual claim on the page is sourced from the upstream package READMEs and
their `test_vectors/`. If an upstream fact changes, change the page.

## Layout

| Path | What it is |
|---|---|
| `index.html` | The single page. Hero, honesty banner, "why now" (harvest-now-decrypt-later), quick start for all three languages, interop matrix, "how it works" (combiner formula, inline SVG diagram, wire-format table, web/native backends), the honest-boundary section, and the ecosystem footer. |
| `style.css` | All page styling. Linked from `index.html` as `<link rel="stylesheet" href="style.css">`. The page has **no** `<style>` block and **no** JavaScript: its only `<script>` is an `application/ld+json` structured-data block. |
| `og-image.png`, `og-image.svg` | The 1200x630 social card **actually referenced** by the `og:image` and `twitter:image` meta tags, which point at `https://skpqc.skworld.io/og-image.png` (repo root). |
| `assets/og-card.png`, `assets/og-card.svg` | Byte-identical duplicates of the two `og-image.*` files at the root. Nothing references these. Kept only so an older shared link does not 404. If you regenerate the card, regenerate **both** copies or the duplicate goes stale silently. |
| `CNAME` | `skpqc.skworld.io`, the GitHub Pages custom domain. |
| `.nojekyll` | Serve files as-is (no Jekyll processing), so `assets/` is served verbatim. |
| `robots.txt`, `sitemap.xml`, `llms.txt` | SEO and AI-crawler signals. `llms.txt` is a plain-text summary aimed at model crawlers. |

Regenerate the social card from its SVG source with:

```bash
# og-image.svg already declares width="1200" height="630".
cairosvg og-image.svg -o og-image.png -W 1200 -H 630
# Keep the unreferenced duplicates in sync, or they go stale silently.
cp og-image.svg assets/og-card.svg
cp og-image.png assets/og-card.png
```

## Deploy

Static site, GitHub Pages served from the `main` branch root (`/`). There is no
build step, no service, no unit file, and no port: nothing about this repository
runs on the SKWorld fleet. Pushing to `main` is the deploy.

`CNAME` sets the custom domain. The Cloudflare DNS record that points
`skpqc.skworld.io` at the GitHub Pages target is configured outside this repo.

## CI

Two workflows, neither of which deploys:

- `.github/workflows/validate.yml` (`validate-site`): asserts `CNAME` and
  `.nojekyll` are present, runs a stdlib `html.parser` well-formedness check over
  every `*.html`, and resolves every internal `href`/`src` plus every in-page
  `#anchor`. External links are deliberately not fetched, so CI never depends on
  the network being up or on a third party staying online.
- `.github/workflows/secret-scan.yml`: the gitleaks **binary** (not the licensed
  action) over the full history, `--redact --exit-code 1`.

## Repository scope

This repo deliberately does **not** carry the full seven-file
`SK_REPO_DOC_STANDARD` set, and deliberately does **not** run the fleet
`docs-check` gate. It has no build, no test suite, no API, no configuration, and
no runtime surface, so `SOP.md` sections 3 to 8 would have nothing true to say,
and a `docs-check` run at tier 1 would start permanently red for files this repo
has no reason to hold. `DOCS_FRESHNESS_STANDARD` section 2 forbids landing a gate
that starts red.

What it does carry: this `README.md`, `SECURITY.md` (which routes reports to the
upstream implementation repos, because the vulnerable code lives there and not
here), `LICENSE`, and the two CI workflows above.

Operational and cryptographic documentation belongs in the implementation repos.

## License

Apache-2.0. See [`LICENSE`](LICENSE). The site footer, the JSON-LD `license`
field, and the upstream packages all declare the same.

Part of the [SKWorld](https://skworld.io/) sovereign infrastructure ecosystem.
