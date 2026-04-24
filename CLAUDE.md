# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running Games

No build step required. Open any game's `index.html` directly in a browser:

```
hot-wheels-video-game/index.html
soccer-video-game/index.html
tennis-video-game/index.html
```

For local development with a simple server: `npx serve .` or `python -m http.server` from the repo root.

## Architecture

Each game is a **single self-contained `index.html`** — no shared code, no external dependencies (beyond Google Fonts). The pattern is:

- **`<style>`** — all CSS inline, including theme variables, animations, and scanline/overlay effects
- **`<canvas>`** — main rendering surface (fullscreen)
- **HTML overlays** — absolutely-positioned divs for title screens, HUD, score panels, modals
- **`<script>`** — all game logic (~700–1,500 lines per game)

### Game Loop Pattern

All three games use the same structure:

```js
const STATE = { TITLE, PLAY, LEVEL_COMPLETE, ... }
let state = STATE.TITLE

function update() { /* physics, AI, state transitions */ }
function draw()   { /* canvas rendering */ }
function gameLoop() {
  update();
  draw();
  requestAnimationFrame(gameLoop);
}
```

### Audio

Sound and music are generated entirely via the **Web Audio API** — no audio files. Each game creates oscillators with square/triangle/sawtooth waves, shaped with gain envelopes, for both chip-tune music tracks and SFX (hit, miss, win, countdown).

### Input

Keyboard (`SPACE`, `ENTER`, arrow keys) plus touch events for mobile. UI buttons use click handlers. Both desktop and touch paths are handled in every game.

### Visual Style

- `image-rendering: pixelated` on the canvas
- CSS pseudo-elements for animated effects (rain, flames, scanlines)
- Google Fonts "Press Start 2P" for all text
- Neon colors on dark backgrounds with thick borders/shadows
- Responsive scaling via `clamp()`

## Games

| Game | File | Notable features |
|------|------|-----------------|
| Hot Wheels Crazy Boys | `hot-wheels-video-game/index.html` | 2-player or vs CPU, best-of-3, trick scoring, music/sound toggles |
| The Experience of Soccer | `soccer-video-game/index.html` | 3-level campaign (Seattle → Mexico City → Beijing), per-level procedural music |
| Seattle Pixel Tennis | `tennis-video-game/index.html` | Single-button 2-player, timing-window mechanics, first-to-6-games scoring |
