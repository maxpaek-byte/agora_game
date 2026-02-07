# CLAUDE.md

## Project Overview

**Snake Master (스네이크 마스터)** is a Korean-language snake game built as a single-file web application. It was created as an educational project for an Agora class. Players control a snake with arrow keys, eating food to grow longer while avoiding walls and their own tail.

## Repository Structure

```
agora_game/
├── CLAUDE.md            # This file - AI assistant guide
├── README.md            # Brief project description (Korean)
└── index.html           # Entire application (HTML + CSS + JS)
```

This is a **single-file application** — all HTML, CSS, and JavaScript live in `index.html` (~528 lines).

### File Layout of `index.html`

| Lines     | Section                        |
|-----------|--------------------------------|
| 1-7       | Document head, meta, fonts     |
| 8-261     | `<style>` block (all CSS)      |
| 263-306   | `<body>` HTML structure         |
| 308-526   | `<script>` block (all JS)      |

## Tech Stack

- **HTML5 Canvas / CSS3 / Vanilla JavaScript (ES6)** — no frameworks or libraries
- **Google Fonts CDN** — Black Han Sans, Noto Sans KR
- **No build tools** — no bundler, transpiler, package manager, or dependencies
- **No tests** — no testing framework is configured
- **No CI/CD** — no workflow files or automation

## How to Run

Open `index.html` directly in any modern web browser. No server, build step, or installation required.

## Game Architecture

### Game States

1. **START** — Modal overlay with instructions, "게임 시작" button
2. **ACTIVE** — Snake moves on canvas; player steers with arrow keys
3. **GAME_OVER** — Modal showing final score, high score, and restart button

### Core Game Logic (JavaScript)

| Function          | Purpose                                              |
|-------------------|------------------------------------------------------|
| `startGame()`     | Initializes state, starts `requestAnimationFrame` loop |
| `update()`        | Moves snake, checks collisions, handles food eating  |
| `draw()`          | Clears canvas, draws grid, food, and snake           |
| `drawSnake()`     | Renders snake segments with neon gradient coloring    |
| `drawFood()`      | Renders pulsing neon food circle                     |
| `drawGrid()`      | Renders subtle background grid lines                 |
| `spawnFood()`     | Places food at random position not overlapping snake  |
| `getSpeed()`      | Returns tick interval; decreases as score increases   |
| `gameOver()`      | Stops loop, updates high score, shows game-over screen |
| `resizeCanvas()`  | Fits canvas to available space on load and resize     |

### Key State Variables

```javascript
snake         // Array of {x, y} grid positions (head is index 0)
direction     // Current movement vector {x, y}
nextDirection // Buffered next direction (prevents 180° reversal)
food          // {x, y} grid position of current food
score         // Player score (+10 per food eaten)
highScore     // Session high score
gameActive    // Boolean flag for game loop
CELL          // Grid cell size in pixels (20)
COLS, ROWS    // Grid dimensions (computed from canvas size)
```

### Speed Progression

| Score     | Tick interval (ms) |
|-----------|--------------------|
| 0–49      | 150                |
| 50–99     | 130                |
| 100–199   | 110                |
| 200–299   | 90                 |
| 300+      | 75                 |

### Visual Design

- **Neon aesthetic** on dark background (`#0a0e27`)
- CSS custom properties: `--neon-pink`, `--neon-blue`, `--neon-purple`, `--neon-yellow`
- CSS keyframe animations: `bgPulse`, `glow`, `fadeIn`, `slideUp`, `pulse`, `titleGlow`
- Snake head glows neon-blue; body gradient transitions from blue to purple
- Food pulses with neon-pink glow
- Canvas has neon-blue border with glow shadow

## Development Conventions

### Code Style

- All code is inline within the single HTML file (no external `.css` or `.js` files)
- CSS comments are in Korean (e.g., `/* 배경 애니메이션 */`, `/* 게임 영역 */`)
- JavaScript has no comments — logic is expressed through descriptive function names
- DOM elements are accessed via `document.getElementById()` cached in top-level `const` variables
- Game loop uses `requestAnimationFrame` with timestamp-based throttling

### Korean Language Context

- UI text is in Korean (instructions, labels, game-over text)
- Font choices (Black Han Sans, Noto Sans KR) support Korean rendering
- The `<html lang="ko">` attribute is set

### When Making Changes

- **Keep it single-file.** Do not split into separate CSS/JS files unless explicitly requested.
- **Preserve the neon visual theme.** Use the existing CSS custom property colors.
- **Maintain Korean UI.** All user-facing text should remain in Korean.
- **No build step.** Changes should work by simply refreshing the browser.
- **Test by opening the file in a browser.** There is no automated test suite.

## Known Limitations

- No mobile/touch input support (arrow keys only)
- No accessibility attributes (ARIA labels, screen reader support)
- No error handling in JavaScript
- No persistent high score storage (resets on page reload)
- Canvas does not resize during active gameplay
