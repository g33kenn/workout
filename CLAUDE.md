# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page workout stopwatch app deployed via GitHub Pages at `workout.pixelvin.com`. No build step, no dependencies to install — just `index.html`.

## Running locally

Open `index.html` directly in a browser. There is no dev server or build process.

## Architecture

Everything lives in `index.html`: inline CSS (`<style>`), HTML structure, and inline JavaScript (`<script>`). There are no external JS files, no bundler, and no framework. Tailwind CSS is loaded from CDN.

### State

All timer state is plain variables at the top of the `<script>` block:
- `running` — whether the interval is active
- `mainTimeElapsed` / `totalTimeElapsed` — millisecond accumulators
- `startTime` / `totalStartTime` — reference timestamps set via `new Date().getTime()`
- `setsCount` — incremented on each "END OF SET" press

### Key functions

| Function | What it does |
|---|---|
| `startTimer()` | Sets `startTime`, starts `setInterval(getShowTime, 10)`, resets `#lapBtn` className |
| `resetTimer()` | Clears interval, zeroes all state and displays, resets `#lapBtn` className |
| `lapTimer()` | Increments set count, resets `mainTimeElapsed`, calls `startTimer()` if not running |
| `getShowTime()` | Called every 10ms; computes and renders both timer displays |
| `toggleTheme()` | Toggles `body.light` class, persists to `localStorage` |

### Styling conventions

- Tailwind utility classes for layout and base styles.
- `body.light #lapBtn` overrides (with `!important`) apply the light-mode palette; these outrank generic class selectors due to ID specificity.
- When JavaScript overwrites `#lapBtn`'s `className`, all three class strings (HTML initial state + `startTimer()` + `resetTimer()`) must stay in sync — they must be identical and always include `w-64`.
- The `#lapBtn` click animation uses `.btn-press:active` (scale + opacity + brief blue flash). Light mode needs its own `body.light #lapBtn.btn-press:active` rule to override the higher-specificity light-mode background.

## Deployment

Push to `main` — GitHub Pages serves the site automatically. No CI, no PR workflow required.
