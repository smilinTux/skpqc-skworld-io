# Security Policy: skpqc-skworld-io

This repository is the **static website** for the sk_pqc post-quantum KEM suite.
It contains a single HTML page, a stylesheet, two images, and three text files.
**It contains no cryptographic code, no server, no dependencies, and no runtime.**

That shapes everything below. The overwhelmingly likely reason you are reading
this page is that you found a vulnerability in the sk_pqc **library**, and the
library does not live here. Send it to the implementation repo instead (see
[Where to report library vulnerabilities](#where-to-report-library-vulnerabilities)).

---

## Honest-claims posture

> **Experimental and unaudited.** sk_pqc is sovereign, independently-built
> software that has had **no** third-party security audit, fuzzing, or formal
> review. It binds vetted primitive libraries (liboqs, `@noble/post-quantum`,
> pyca `cryptography`) and ships known-answer vectors. A passing test suite
> proves interop, **not** the absence of side channels or protocol flaws.
> Treat it as research-grade and review it yourself before production use.

Scoped claims, per the sk-standards
[CRYPTOGRAPHY_STANDARD](https://github.com/smilinTux/sk-standards/blob/main/standards/CRYPTOGRAPHY_STANDARD.md):

- The suite is **hybrid X25519 + ML-KEM-768** (`x25519-mlkem768`), combined as
  `HKDF-SHA256(X25519_ss || MLKEM768_ss)`. Concatenate-then-KDF, never XOR,
  never pure post-quantum.
- **Hybrid means secure if EITHER leg holds.** Confidentiality survives as long
  as either the classical leg or the post-quantum leg is unbroken.
- ML-KEM-768 is standardized as **FIPS 203**. This is the **-768** tier, not the
  CNSA-2.0 ML-KEM-1024 tier, and the site does not claim otherwise.
- **KEM only.** There are no signatures. FIPS 204 (ML-DSA) is referenced as
  future work, not implemented.
- **AES-256 and SHA-256 are not quantum-broken.** They are symmetric and
  Grover-only. Any text implying otherwise is a defect, not a warning.
- The defensible words are **post-quantum** and **quantum-resistant**. The words
  "quantum-proof", "quantum-safe", and "unbreakable" are never used as a claim.

---

## Scope

### In scope for this repository

- **A false, unscoped, or overclaimed cryptographic statement on the published
  page.** For a cryptography site this is a security defect, not a typo: an
  overclaim tells a reader they are safe on a surface where they are not. That
  includes dropping the hybrid qualifier, implying signatures exist, claiming
  the -1024 tier, implying AES-256 is quantum-broken, or using any of the
  forbidden words above.
- **Anything that lets a third party change what `skpqc.skworld.io` serves.**
  Custom-domain or subdomain takeover of the `CNAME` target, a GitHub Pages
  configuration weakness, or a workflow in `.github/workflows/` that could be
  induced to write to the deployed branch.
- **A malicious or unexpected sub-resource loaded by the page.** As of
  2026-08-15 the page loads exactly one sub-resource, the same-origin
  `style.css`. There is no third-party script, stylesheet, font, or image, no
  analytics, no CDN, and no cookie. The favicon is an inline `data:` URI. Every
  other external URL on the page is an `<a href>` the reader must click. A pull
  request that adds a third-party loaded resource is in scope.
- **A secret committed to this repository.** There should never be one. The
  `secret-scan` workflow scans the full history on every push.

### Out of scope for this repository

- **Every cryptographic vulnerability in sk_pqc itself**: the combiner, the wire
  format, the FFI bindings, the web backend, key handling, constant-time
  behaviour, test-vector correctness. That code is in the implementation repos.
  Report it there.
- **Vulnerabilities in the bound primitive libraries** (liboqs,
  `@noble/post-quantum`, pyca `cryptography`). Report upstream to those projects.
- **Vulnerabilities in GitHub Pages, GitHub Actions, or Cloudflare themselves.**
  Report those to the respective vendor.
- **Key custody, passphrase storage, and endpoint security** for anyone using
  sk_pqc. Not a property of a website.
- Cosmetic issues, broken external links, SEO, and accessibility. Please open a
  normal public issue for those.

---

## Where to report library vulnerabilities

If the issue is in sk_pqc's cryptography or code, report it against the
implementation you found it in, **not** here:

| Implementation | Repository |
|---|---|
| Python (`pip install sk-pqc`) | <https://github.com/smilinTux/sk-pqc-py> |
| Dart / Flutter (`dart pub add sk_pqc`) | <https://github.com/smilinTux/sk-pqc-dart> |
| Rust (`cargo add sk-pqc`) | <https://github.com/smilinTux/sk-pqc-rs> |
| OpenPGP / PQC signing library | <https://github.com/smilinTux/sk_pgp> |

If the defect is shared across implementations (a wire-format or combiner
problem, for example), report it once against `sk-pqc-py` and say so. Do not
open the same issue four times: coordinating a fix across three languages is
easier from one report.

> Historical note: `github.com/smilinTux/sk_pqc` still resolves through a GitHub
> rename redirect to `sk-pqc-dart`. Use the real names above.

---

## Reporting a vulnerability in this repository

**Do not open a public GitHub issue for a security vulnerability.**

- **Primary:** GitHub **private vulnerability reporting**. Go to the
  [Security tab of `smilinTux/skpqc-skworld-io`](https://github.com/smilinTux/skpqc-skworld-io/security)
  and choose "Report a vulnerability". This channel is enabled on this
  repository.
- **Secondary:** contact the maintainers (smilinTux / SKWorld) via the address
  published on the GitHub organization profile.

Please include the affected URL or file, what the page currently says, what it
should say, and why the current wording is misleading or exploitable.

We aim to **acknowledge within 72 hours**. For a content correction the fix is
usually a same-day commit, because deploying this site is a push to `main`. For
anything requiring an upstream change we will coordinate a disclosure date with
the implementation repo.

**Safe harbour:** good-faith research conducted under coordinated disclosure will
not be pursued. Please do not run automated scanners against
`skpqc.skworld.io`; it is a static page on GitHub Pages and there is nothing
behind it to find. Credit is given unless you ask otherwise.

---

## Supported versions

This is a rolling static site, not a released artifact. There are no versions to
support.

| Ref | Status |
|---|---|
| `main` | The deployed site. GitHub Pages serves this branch's root. Fixes land here. |
| Any other branch or tag | Not deployed, not supported. |

For the **library's** supported versions, see the `SECURITY.md` in each
implementation repository.

---

**License:** Apache-2.0 (see [`LICENSE`](LICENSE)).
**Standards:** FIPS 203 (ML-KEM); RFC 7748 (X25519); RFC 5869 (HKDF);
ISO/IEC 29147 and 30111 (vulnerability disclosure).
