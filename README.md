# Dr. Nagakumari — practice site

Static site. No build step: `index.html` plus `assets/`.

    open index.html          # local preview
    python3 -m http.server   # or serve the folder

Live copy (private Claude artifact): https://claude.ai/artifact/FN65BzPK4zYDq42VMjswpb

## Deploying

Static site, no build step. On Vercel choose framework preset **Other**,
leave build command empty and output directory as the repo root.
`vercel.json` sets long-lived caching on `/assets` and clean URLs.

## Reels play in place

Clicking a reel opens Instagram's own embedded player in a lightbox rather
than navigating away. The player needs `https://www.instagram.com/embed.js`,
which only loads on a real domain — inside a sandboxed preview it is blocked,
so after 3.5 seconds the lightbox falls back to the cover image plus an
"Open on Instagram" button. That fallback is expected in previews, not a bug.

Reel covers are stored locally because Instagram's CDN links expire. If a
cover ever needs refreshing, re-download it rather than hotlinking.

## Before it goes public

### 1. The hero photograph

The hero expects the herbal flat-lay at:

    assets/img/hero-herbs.jpg

Save it there and it appears automatically — no code change needed. Until
then the hero paints a layered green gradient in the same palette, so the
page never looks broken. Use a wide landscape crop (1800px or more) and keep
the file under ~600KB.

### 2. Two placeholders

In the `#consult` section of `index.html`, phone and email render as
"to be added". Replace both `<span class="pending">` lines with the real
details, e.g.

    <span class="v"><a href="tel:+919999999999">+91 99999 99999</a></span>

## Reels

Five reels are linked from the `#reels` section. Covers were pulled from the
public Instagram account and stored locally in `assets/reels/` — Instagram's
CDN links expire, so do not hotlink them.

| Cover file | Reel | Plays (Sept 2026) |
|---|---|---|
| superfood-swaps.jpg | instagram.com/reel/DY7leU7veKG | 5,581 |
| surya-namaskar.jpg | instagram.com/reel/DZdDe3NTAAL | 5,558 |
| simple-reset.jpg | instagram.com/reel/DZF4ks7Pig- | 3,290 |
| bloating.jpg | instagram.com/reel/DbYGN8ATL-P | 1,284 |
| night-habits.jpg | instagram.com/reel/DZ-jCOPTUtc | 1,035 |

Three spare covers are also in `assets/reels/` (sleep-breathing, kashayam,
late-night-eating) if you want to swap one in. Play counts are hardcoded —
edit the `.play` spans when you refresh them.

## Design notes

Modelled on the calm, tonal approach of beefreshhoney.com:

* **One accent only** — honey amber `#C8871F`. Resist adding a second; the
  earlier multi-colour version is what made the page feel synthetic.
* **Palette** — oat `#F2EFE7`, cream `#FBF8F1`, bark `#1C2318` for the two
  dark chapters. Green comes from the hero photograph, not from tints.
* **Type** — Outfit (display) over Figtree (body). No third face.
* **Space is the design.** Section padding is `--pad`, clamp(94px, 11vw,
  168px). If a section feels crowded, add space before adding anything else.
* **Two dark chapters** (the method, the philosophy) break up the light
  ground. Keep it to two.
* Headings inside `.sec-dark` are white via `.sec-dark .method-head h2` —
  do not move that colour onto `.method-head` alone or light-section
  headings turn white too.

The day rail, the myth rows and the reel copy are all drawn from her own
posts.

* **Phone overrides go at the END of the stylesheet.** A `@media` block does
  not beat a later rule of equal specificity, so mobile overrides placed
  near the top get silently undone. The `@media (max-width:560px)` block
  just above the reduced-motion block is where they belong.
* Verify mobile with DevTools emulation, not headless `--window-size`:
  headless ignores narrow widths and crops a desktop layout, which looks
  like a broken page when it is not.
