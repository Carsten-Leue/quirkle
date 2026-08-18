# Agent instructions for this repo

This is a single-file scoreboard app (`index.html`, no build step, no
dependencies) for two card/tile games, **Qwirkle** and **Wizard**, built
specifically to run inside the **Kindle Paperwhite 7th generation's**
"Experimental Browser". Read this file before making changes.

## 1. Strict Kindle Paperwhite 7th-gen compatibility

The target browser is an old, low-power WebKit build with no modern engine
updates and no way to upgrade it. Every change must keep working there —
don't assume "it works in a current desktop browser" is good enough.

**CSS**

- No CSS Grid, no Flexbox, no `clamp()`, no CSS custom properties
  (variables), no `gap`. Layout is built with floats and
  `display:table`/`table-cell` (CSS2.1) — both are safe here.
- `@media` queries work and are fine to use.
- No animations, transitions, or gradients. The e-ink display can't
  refresh fast enough to render them usefully, and partial-refresh
  ghosting makes moving/fading content look broken rather than smooth.
  Keep every state change an instant, discrete redraw.
- Stay pure black-and-white / high-contrast. Don't introduce color as
  the only way to convey information — e-ink has no color and greyscale
  contrast is weak.
- Any sizing that needs to "fill the page" (heights, font sizes that
  scale with available space) must be computed in JavaScript with plain
  `px` values set via inline styles, not `vh`/`vw`/`clamp()`.

**JavaScript**

- ES5 only: `var` (no `let`/`const`), `function` expressions (no arrow
  functions), string concatenation (no template literals), no
  destructuring, no spread/rest, no classes, no `Array.prototype`
  methods newer than ES5 (`forEach`/`map`/`splice`/`filter` are fine;
  don't reach for `includes`, `find`, `Object.assign`, optional
  chaining, nullish coalescing, etc.).
- No `fetch`, no Service Workers, no web app manifest / install-to-
  homescreen — assume none of that is available and don't rely on it.
- Wrap every `localStorage` access in `try`/`catch`; storage can be
  unavailable or throw, and the app must keep working (in-memory) if so.
- Don't rely on native `<input type="number">` spinners or on `:hover`
  states — the device is a touchscreen with no reliable hover, and
  native number inputs behave inconsistently on this browser. Prefer
  the app's own tap-target buttons (steppers, number-grid pickers).

**Touch & viewport**

- Tap targets must stay large — this is a touchscreen with lower
  precision than a phone, used by hands, sometimes through a case.
  Don't shrink existing button heights/paddings without a real reason.
- The `viewport` meta tag disables pinch-zoom (`user-scalable=no`) on
  purpose, so layouts must be legible at the device's own size without
  relying on the user zooming in.
- Assume a portrait ~758×1024 CSS-pixel viewport as the primary target
  (what the Experimental Browser reports on this device), but the app
  should still degrade reasonably on other widths — see the existing
  `@media (max-width: 480px)` rules for an example of adapting instead
  of just breaking.

**Testing changes**

- The real device/browser isn't available in this environment. Use the
  pre-installed Chromium (`/opt/pw-browsers/chromium` via Playwright)
  as an approximation, and manually re-check that nothing in the diff
  reintroduces Grid/Flexbox/ES6/`clamp()`/animations.
- Test at the ~758×1024 primary viewport *and* at a narrow phone-sized
  width (e.g. 380px, 320px) — see `layout()` in both game modules for
  why: card sizing and text width are computed relative to the
  available viewport, and multi-column layouts get tight fast.

## 2. Show only what's relevant to the current step

Both games are built around **progressive disclosure**: at any point,
the screen shows only the controls needed for the decision the player is
making *right now*, not everything at once. This keeps tap targets big
and readable on a small screen instead of cramming a dense dashboard
onto a 6-inch display. When adding or changing UI, keep following this
pattern rather than exposing every control all the time:

- **Wizard's round** is split into three phases (`bid` → `tricks` →
  `result`) that each render their own screen. Bidding further shows
  only the one player whose turn it is (name + a number-grid picker),
  not all players' pickers at once — other players collapse into a
  small "so far" overview line until it's their turn or you tap to
  correct them. The round's score is only shown large in the `result`
  phase; the two input phases show nothing or a small recap line, never
  the computed score.
- Player management (add/remove player, rename) is only exposed where
  it's safe and relevant — before the first bid of a game, or on the
  `result` screen between rounds — not while someone is mid-input.
- Prefer replacing a screen's content over stacking more sections onto
  it. If a new feature only matters in one phase, gate it behind that
  phase's render function rather than always rendering it and hiding it
  with CSS — dead markup still costs layout/measurement time on this
  browser and clutters `layout()`'s natural-height measurements.
- When something *does* need to always be reachable regardless of phase
  (e.g. "New Game"), put it in the persistent header as a small link
  rather than growing the primary action area.

Before adding a new element to a card or phase, ask: does the player
need this to make *this* decision, or does it belong on the next/
previous screen instead? If it's the latter, move it.
