# CLAUDE.md

Guidance for working in this repo.

## What this is

`index.html` is the entire project — a single static page, no build step, no backend, built to
[`anoint-genesis-scent-build-spec.md`](./anoint-genesis-scent-build-spec.md). See that file for the
actual brand brief; this file only covers implementation decisions and gotchas not obvious from
the spec itself.

## The tone-transition mechanic

Every `<section>` that should drive the page's background carries a `data-tone` attribute (`bone`,
`amber`, `teal`, `tobacco`, or `ink`). An `IntersectionObserver` with `rootMargin: '-45% 0px -45%
0px'` (so a section only counts as "active" once it's roughly centered in the viewport, not just
grazing an edge) sets `document.body.dataset.tone` to match, which drives a CSS custom-property
swap (`--tone`/`--tone-text`) with a 700ms transition on `background-color`/`color`.

**Watch the selector**: the observer only watches `section[data-tone]`, not the bare `[data-tone]`.
`<body>` itself carries a `data-tone="ink"` attribute too (as the page's initial/fallback state),
so the universal selector would pull `body` into the same observed set — its own intersection
state (which is basically "always intersecting, it's the whole page") then fires `setActive()`
spuriously alongside the real sections. Keep the `section` qualifier if you touch this.

**Hero and closing are not fragrances.** Both carry `data-tone="ink"` (matching Ascent
Singularity's own tone, which "doubles as" the base background per the spec) so the page bg looks
right at those two bookends — but the rail's 5 orbs track progress through the 5 *fragrances*
specifically, not through tones. `setActive(tone, isFragrance)` takes a second argument for exactly
this reason: hero/closing pass `isFragrance: false`, which clears every orb, rather than lighting
the "ink" orb the way Ascent's own section correctly does. Without that distinction, the last orb
looks lit before a visitor has scrolled at all, which reads as backwards.

## Known limitation: no purchase flow

The spec's CTA is deliberately vague ("e.g. 'View the Collection' / 'Get Notified'") and the layout
section never describes a cart or checkout — this is a brand/story page only. `anoint-store.html`
(also in this Downloads-derived material, not copied into this repo) is a different, older, more
generic-looking prototype with a cart + Nostr login + BTC checkout for a single bundled $40
product — it predates this spec, doesn't reflect the 5-fragrance-as-distinct-products framing, and
wasn't used as a basis for this build. If a real checkout is wanted later, treat that as new scope
to spec out properly rather than grafting the old prototype's approach onto this page.

## Note pyramids are reference-fragrance research, not a formula

Each fragrance's Top/Middle/Base notes and its "In the spirit of" comparable (in `index.html`'s
five `.fragrance` sections) come from the spec's table, which was itself sourced from Fragrantica's
public listings for each named reference fragrance (accessed Aug 2026) — see
`anoint-genesis-scent-build-spec.md`. They're **directional inspiration for the brief and the
page's sensory copy, not an actual formulation** — an actual perfumer/fragrance house still needs
to develop each juice's real formula. Updated once already (2026-08-08) when a revised spec swapped
in Fragrantica-sourced notes for what had been a more invented-feeling list, and changed 3 of the 5
comparables (Prophet → Bleu de Chanel EDP, Consensus → Tom Ford Tobacco Vanille, Ascent → Nishane
Hacivat); Dawn Aurora's comparable stayed Creed Aventus Absolu across both versions. If the spec
changes again, update all three of: the copied spec file, `index.html`'s five `notes-grid`s, and
this note.

## Design

Deliberately avoids both the cream/terracotta AI-default palette and a near-black-with-one-neon
-accent palette — the spec is explicit that all 5 named tones need to actually appear, not just be
described. Fraunces (display serif, "inscriptional/liturgical") carries headlines and fragrance
names; Space Grotesk (geometric sans, "protocol/ledger") carries body copy — the two halves of the
brand's sacred-meets-Bitcoin-culture fusion the spec calls for.

`prefers-reduced-motion: reduce` disables the background-color transition (snaps instead),
disables smooth scrolling, and stops the hero's scroll-cue pulse animation — don't add new motion
without gating it the same way.
