# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Project overview

Single-file HTML5 canvas application (`love-heart.html`). One animated heart made of repeating text words, fully configurable via a settings panel. No build step, no dependencies — open directly in a browser.

## Running the project

Open `love-heart.html` directly in a browser. No dev server, no build, no tests.

## Architecture

Everything lives in one file:

- **CSS** — dark theme: fixed settings toggle button, slide-in right settings panel, one full-screen canvas, star background canvas.
- **Global `cfg` object** — single source of truth for all settings. `applyConfig()` reads DOM inputs and writes to `cfg`.
- **Heart math** — `hx(t)` / `hy(t)` parametric heart curve (`16sin³t` / `13cos t − 5cos 2t − 2cos 3t − cos 4t`).
- **One draw function** `drawHeart(state)` — renders one frame using `cfg`. Supports all display styles, rotation styles, wave types via switch/if logic inside.
- **Stars** — independent `requestAnimationFrame` loop on a separate canvas layer behind the heart.
- **Main loop** — single `requestAnimationFrame` loop calling `drawHeart`.

## cfg object fields

| Field | Type | Description |
|---|---|---|
| text | string | Word/phrase on heart |
| title | string | Center heading |
| sub | string | Center subheading |
| showTitle | bool | Show/hide center text |
| font | string | Font family |
| fontsize | number | px |
| fontStyle | string | bold / normal / italic |
| color1 | hex | Main text color |
| colorGlow | hex | Glow/halo color |
| colorTitle | hex | Center title color |
| bg | hex | Background color |
| glowIntensity | number | 0–100 |
| scale | number | 0.5–2.0 |
| points | number | 30–200 |
| offsetX | number | -0.5 to 0.5 (fraction of W) |
| offsetY | number | -0.5 to 0.5 (fraction of H) |
| stretchY | number | 0.5–1.5 |
| rotSpeed | number | 0–100 |
| rotDir | number | 1 or -1 |
| rotStyle | string | uniform / pendulum / pulse / chaotic / spiral |
| waveSpeed | number | 0–100 |
| waveType | string | smooth / flash / radial / double / random |
| displayStyle | string | glow / ghost / volume3d / outline / double |
| bgBlur | number | 0–20 |
| opacity | number | 0.2–1.0 |
| starsCount | number | 0–500 |
| starsSpeed | number | 0–100 |
| starsColor | hex | Star color |
| starsOn | bool | Stars on/off |

## Key conventions

- Canvas sized with `devicePixelRatio` for HiDPI.
- Colors stored as hex in `cfg`, converted via `hexToRgb()` at draw time.
- No localStorage, no URL params — settings are session-only.
- Settings panel slides in from right; heart canvas always fills full screen behind it.
- All settings apply instantly in real time via `applyConfig()` called on every input event.