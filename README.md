# Punktetafel — Qwirkle & Wizard Scoreboard

A single-page, e-ink-friendly scoreboard for **Qwirkle** and **Wizard**, built
to run in the Kindle Paperwhite's Experimental Browser (no jailbreak
required). The whole app is one `index.html` file — no build step, no
dependencies, no framework.

Everything is in German, since that's the language of the group this was
built for.

- Pure black-and-white, high-contrast layout — no gradients or animation, so it stays crisp on e-ink and doesn't ghost.
- Large tap targets for the Kindle's touchscreen.
- A start screen lets you pick **Qwirkle** or **Wizard**; your choice is remembered, so the app reopens straight into the last game you played.
- Each player gets a unique geometric symbol (circle, square, diamond, star, cross, clover) next to their name, so players stay easy to tell apart at a glance.
- Scores autosave to the browser's local storage, so a reload or a moment of Wi-Fi dropping out won't lose the game. Qwirkle and Wizard each keep their own separate save, so switching games never overwrites the other's scores.

## Qwirkle

- +1 / -1 / +3 steppers, a **Qwirkle +6** button, and a one-off **Schluss +6** button for emptying your rack.
- Add or remove up to 6 players, edit names inline.

## Wizard

- Supports 3–6 players, adjustable before the game starts (or between rounds).
- Round count is derived from the deck automatically (60 cards ÷ number of players).
- Each round runs through three focused phases, so the screen only ever shows what that phase needs:
  1. **Tipp abgeben** — players bid one at a time, in turn order. Only the player currently bidding sees their name and a number grid (0 up to this round's trick count); everyone else appears as a small line in a "bids so far" overview below, which you can tap to correct an earlier bid. The last player to bid can't pick the number that would make every bid add up exactly to the tricks available (the classic Wizard "hook" rule).
  2. **Stiche eintragen** — every player enters the tricks they actually won, from the same kind of number grid.
  3. **Ergebnis** — the round's score and the new total are shown large; the previous two phases show little or nothing of this, to keep the focus on the current input.
- Scoring: 20 + 10 points per trick for an exact bid, otherwise −10 points per trick of difference.
- After the last round, the player with the highest total is marked as the winner.

## Put it on GitHub Pages

1. Create a new **public** repository on GitHub (e.g. `qwirkle-scoreboard`).
2. Upload `index.html` (drag and drop on the repo's "Add file → Upload files" page, or `git push`).
3. Go to **Settings → Pages**, set Source to the `main` branch, root folder, and save.
4. GitHub will give you a URL like `https://yourusername.github.io/qwirkle-scoreboard/`.

## Open it on your Kindle

1. On the Kindle, make sure Wi-Fi is on.
2. Home screen → tap the search icon → tap the browser/globe icon (or Settings → search → "Experimental Browser" depending on firmware).
3. Enter your GitHub Pages URL.
4. Bookmark the page for quick access next game night.

## Notes

- No build step, no dependencies — it's one HTML file.
- Works offline once loaded, as long as you don't reload the page (Kindle browser doesn't cache reliably).
- Written for the Kindle Paperwhite's old WebKit browser: no CSS Grid/Flexbox/`clamp()`, layout uses floats and `display:table`/`table-cell`; no ES6 in the JavaScript (`var` only, no arrow functions, no template literals).
