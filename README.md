# ANOINT : GENESIS SCENT

A single-page brand site for a 5-fragrance men's collection, tracing a masculine journey in five
stages — vision, awakening, flow, maturation, transcendence — through a scroll-driven color arc:
**White → Amber → Teal → Tobacco → Black**.

Built from [`anoint-genesis-scent-build-spec.md`](./anoint-genesis-scent-build-spec.md) — see that
file for the full brand brief (fragrance notes, hooks, design direction).

## Running it

No install, no build step. Either open `index.html` directly, or serve it locally:

```
python3 -m http.server 8000
```

then visit `http://localhost:8000/`.

## The signature mechanic

A vertical "anointing thread" runs down the page (fixed left rail on desktop, inline dots above
each section's marker on mobile) with an orb per fragrance. As you scroll into a fragrance's
section, the page background transitions to that fragrance's tone color and the thread's
corresponding orb lights up gold — the color arc is something you travel through, not a swatch
you're just told about. See `CLAUDE.md` for the implementation details and a couple of bugs that
came up building it.

## Status

This is the brand/story page only — no cart, no checkout, no purchase flow. (There's an older,
unrelated prototype for that, `anoint-store.html`, that predates this spec and uses a different,
more generic visual style; it isn't part of this build and wasn't used as a basis for it.)
