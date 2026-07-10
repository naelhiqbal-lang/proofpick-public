# ProofPick

**A trust‑weighted “Smart Score” for Amazon — shop by proof, not hype.**

A `5.0` from 3 reviews looks better than a `4.6` from 4,000 — but it isn’t. ProofPick
is a Manifest V3 browser extension that reads every product’s rating **and how much
evidence backs it**, then shows a single, transparent Smart Score (0–100) with the
reasons behind it — right on the Amazon page, as you shop. No hype, no black box,
and nothing about you leaves your browser.

![Smart Score badges on search results](screenshots/search-badges.png)

<sub>Product images and prices above are placeholders in a demo layout — the extension renders these badges on the real store page.</sub>

---

<p>
  <img alt="Manifest V3" src="https://img.shields.io/badge/Chrome-Manifest%20V3-5257c9">
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-strict-3178c6">
  <img alt="Tests" src="https://img.shields.io/badge/tests-574%20passing-3f6b53">
  <img alt="Privacy" src="https://img.shields.io/badge/privacy-by%20design-3f6b53">
  <img alt="Remote code" src="https://img.shields.io/badge/remote%20code-none-9e4f34">
</p>

> **Note on this repository.** This is the public showcase for ProofPick. The
> source code is kept private while the product is prepared for release; this repo
> is the home for its product overview, screenshots, privacy summary, and feedback.
> Engineers/reviewers who’d like a deeper look or a code walkthrough are welcome to
> [open an issue](https://github.com/naelhiqbal-lang/proofpick-public/issues/new).

---

## What it does

- **Smart Score on every product** — a 0–100 score on each search result and product
  page that weights the star rating by how much review evidence stands behind it.
- **Transparent reasons, never a black box** — every score expands into plain‑English
  “why”, plus evidence and confidence chips. A perfect rating from a handful of buyers
  is called out, not celebrated.
- **Smart Sort** — reorder search results by real evidence with one toggle; sponsored
  results are always grouped separately, never mixed into the merit ranking.
- **Product trust panel** — a full breakdown on the product page, with a fractional
  star display and honest states when a rating is missing.

![Product‑page trust panel](screenshots/product-panel.png)

- **Compare** — a side‑by‑side of the alternatives a listing already surfaces, scored
  on trust, evidence and rating, with a clear “our take”.

![Compare view](screenshots/compare.png)

- **Works wherever you shop on Amazon** — 17 marketplaces (UK, US, DE, FR, IT, ES, NL,
  SE, BR, MX, JP, IN, AE, SG, CA, AU, IE) with locale‑aware rating and number parsing.
- **On the roadmap** — a Deal Score (price‑history context) and opt‑in AI Review
  Intelligence that summarises what buyers actually say.

## How the Smart Score works

Star averages are noisy and easy to game. ProofPick treats each rating as *evidence*:

1. **Shrink toward a sensible prior.** With few reviews, the score is pulled toward a
   neutral baseline; as real reviews accumulate, it earns its way up. This is why a
   5.0 from 3 reviews scores below a 4.6 from thousands.
2. **Weight by statistical confidence.** A lower‑confidence bound on the rating means
   the score reflects how *sure* we can be, not just the raw average.
3. **Explain everything.** The score always comes with its reasons and an honest
   confidence level — and when there’s no rating, ProofPick says so instead of guessing.

The result is a number you can actually trust, built from transparent statistics —
not sentiment, not paid placement, not fabricated “authenticity” claims.

## Engineering highlights

- **Manifest V3, TypeScript (strict).** Fully type‑safe; a service worker with no DOM
  dependencies, verified to start cleanly in a real Chromium runtime.
- **Layered architecture with an enforced dependency direction** — a pure core, a
  retailer‑adapter pattern (adding a store is “add one adapter”), an isolated scoring
  engine, a browser‑seam platform layer, and four thin surfaces (popup, options,
  background, content).
- **On‑page UI in Shadow DOM** — every injected badge and panel is fully style‑isolated
  from the host page, and vice‑versa.
- **574 automated tests** across parsing, scoring, UI and safety, plus a build‑integrity
  checker and a real‑Chrome verification harness that loads the packaged extension and
  asserts the worker registers and no page script is ever disturbed.
- **Locked down by design** — a strict Content‑Security‑Policy, **no remote code, no
  `eval`**, and Smart Sort that only ever repositions validated product cards (it never
  clones, injects, or re‑executes a page’s scripts).
- **Least privilege** — only the `storage` and `activeTab` permissions; no browsing or
  purchase history, no analytics, no tracking.

## Tech stack

TypeScript · React (popup & options) · Vite + CRXJS · Vitest · Shadow DOM · Chrome
Manifest V3 · Playwright (real‑Chrome verification).

## Privacy

ProofPick is **private by design**:

- **No personal data leaves your browser.** It reads the public product/rating markup
  already on the page to compute a score locally — that’s it.
- **No browsing or purchase history, no accounts, no analytics, no trackers, no ads,
  no affiliate links.**
- **No remote code, ever.** Everything runs from the extension’s own bundled files
  under a strict Content‑Security‑Policy.
- **Least privilege.** Only `storage` (to remember your settings) and `activeTab` (so
  the popup can read the current tab when you open it).

Optional, clearly‑gated future features (e.g. AI review summaries) are strictly
opt‑in and off by default — nothing is read or sent without an explicit switch.

## Status

In active development and preparing for the Chrome Web Store. This repository tracks
the public product story; the implementation is private for now.

## Feedback

Found a bug, have an idea, or want a technical walkthrough?
**[Open an issue »](https://github.com/naelhiqbal-lang/proofpick-public/issues/new)**

---

<sub>ProofPick is an independent project and is not affiliated with, endorsed by, or
sponsored by Amazon. “Amazon” is a trademark of its respective owner.</sub>
