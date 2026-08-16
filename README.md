# Qwirkle Scoreboard

A single-page, e-ink-friendly scoreboard for Qwirkle, built to run in the
Kindle Paperwhite's Experimental Browser (no jailbreak required).

- Pure black-and-white, high-contrast layout — no gradients or animation, so it stays crisp on e-ink and doesn't ghost.
- Large tap targets for the Kindle's touchscreen.
- +1 / -1 / +3 steppers, a **Qwirkle +6** button, and a one-off **Final +6** button for emptying your rack.
- Add or remove up to 6 players, edit names inline.
- Scores autosave to the browser's local storage, so a reload or a moment of Wi-Fi dropping out won't lose the game.

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
