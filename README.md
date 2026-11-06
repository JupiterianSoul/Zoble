# Zoble

A pile of small web games and toys. Things to poke, break, draw, guess and waste a good afternoon on. Live at zoble.pages.dev.

Everything is plain HTML, CSS and JS. No build step, no frameworks, nothing loaded from the network. Each game lives in its own folder under `site/games/<slug>/` and shares a tiny runtime (`site/shared.js` and `site/shared.css`) that adds the top bar, dark mode, mute, a random-game button and a Simple / Advanced switch.

## Running it

Any static server pointed at `site/` works:

```
npm run serve
```

## Checks

```
npm install
npx playwright install chromium
npm run check
```

`sweep` makes sure there are no code comments and no em dashes anywhere (vendored and minified files are skipped). `smoke` opens the hub and every game at desktop and phone size and fails on errors, missing files, sideways scrolling or a missing top bar. To check a single game: `node tools/smoke.mjs <slug>`.

## Adding a game

1. Make `site/games/<slug>/index.html` and a `thumb.svg` (`viewBox="0 0 400 300"`, flat and colourful, under about 6 KB). Add `game.js`, `style.css` or `data.js` if the page is big.
2. Link `../../shared.css`, `../../games.js` and `../../shared.js`, and put `data-game="<slug>"` on the body. Add `data-modes` to get the Simple / Advanced switch, and read `Curio.mode` when the game starts.
3. Add an entry to `site/games.js`.
4. Run the checks.

The shared runtime exposes `window.Curio`: namespaced storage (`store.get`, `store.set`), personal bests (`best`, `getBest`), `beep`, `toast`, `modal`, `confetti`, and small helpers like `rand`, `pick` and `shuffle`. The CSS tokens (`--bg`, `--surface`, `--ink`, `--accent` and friends) make light and dark mode work, so use them instead of fixed colours.

House rules:

- Works with mouse, keyboard and touch. Phones at 390px wide must be playable with no sideways scrolling.
- Works in light and dark mode, no console errors, no `alert()` or `prompt()`, loops pause when the tab is hidden.
- Each game gets its own look, sounds and voice while still feeling part of the site.
- Libraries are fine if they are permissively licensed and vendored into `site/vendor/<name>/` with their license. The live site never loads anything from another server.
- No code comments, and no em dash character anywhere.

## Archive

`archive/` holds earlier versions of the site that are no longer deployed: Zob's computer (`zobos`, a desk, a CRT and a small desktop OS) and the retro Zoble 98 desktop (`zoble98`).

## Hosting

Cloudflare Pages, connected to this repo. No build command, output directory `site`. Every push to `main` goes live.
