# CLAUDE.md

## Project Overview

**Typing Master (타이핑 마스터)** is a Korean-language typing speed game built as a single-file web application. It was created as an educational project for an Agora class. Players type falling Korean words before they reach the bottom of the screen.

## Repository Structure

```
agora_game/
├── CLAUDE.md            # This file - AI assistant guide
├── README.md            # Brief project description (Korean)
└── index.html     # Entire application (HTML + CSS + JS)
```

This is a **single-file application** — all HTML, CSS, and JavaScript live in `index.html` (~671 lines).

### File Layout of `index.html`

| Lines     | Section                        |
|-----------|--------------------------------|
| 1-7       | Document head, meta, fonts     |
| 8-413     | `<style>` block (all CSS)      |
| 415-471   | `<body>` HTML structure         |
| 473-669   | `<script>` block (all JS)      |

## Tech Stack

- **HTML5 / CSS3 / Vanilla JavaScript (ES6)** — no frameworks or libraries
- **Google Fonts CDN** — Black Han Sans, Noto Sans KR
- **No build tools** — no bundler, transpiler, package manager, or dependencies
- **No tests** — no testing framework is configured
- **No CI/CD** — no workflow files or automation

## How to Run

Open `index.html` directly in any modern web browser. No server, build step, or installation required.

## Game Architecture

### Game States

1. **START** — Modal overlay with instructions, "게임 시작" button
2. **ACTIVE** — Words fall from top; player types to destroy them
3. **GAME_OVER** — Modal showing final score and restart button

### Core Game Logic (JavaScript)

| Function          | Purpose                                              |
|-------------------|------------------------------------------------------|
| `startGame()`     | Initializes state, starts spawn and check intervals  |
| `spawnWord()`     | Creates a random falling word element in the game area |
| `checkWords()`    | Runs every 100ms; removes words that reached bottom, triggers `loseLife()` |
| `wordInput` listener | Matches typed input against active falling words   |
| `destroyWord()`   | Removes matched word element, triggers particle effect |
| `levelUp()`       | Increases level, speeds up word fall and spawn rate   |
| `loseLife()`      | Decrements lives; calls `gameOver()` at 0 lives      |
| `gameOver()`      | Stops intervals, disables input, shows game-over screen |
| `restartGame()`   | Clears game area, re-calls `startGame()`             |

### Key State Variables

```javascript
score       // Player score (10 * level per word)
level       // Current level (increments every 100 points)
lives       // 3 lives; lose one when a word hits bottom
gameActive  // Boolean flag for game loop
fallingWords // Array of {element, word, startTime} objects
wordSpeed   // CSS animation duration in ms (starts 3000, min 1500)
spawnRate   // Interval between spawns in ms (starts 2000, min 1000)
```

### Word Pool

115 Korean words across categories: fruits, electronics, places, food, sports, entertainment, people, weather, animals, school supplies, vehicles, geography, colors, instruments, emotions. Defined as a flat array at the top of the `<script>` block.

### Visual Design

- **Neon aesthetic** on dark background (`#0a0e27`)
- CSS custom properties: `--neon-pink`, `--neon-blue`, `--neon-purple`, `--neon-yellow`
- CSS keyframe animations: `fall`, `bgPulse`, `glow`, `fadeIn`, `slideUp`, `pulse`, `particleFade`, `levelUpAnim`, `titleGlow`
- Particle burst effect on word destruction (12 particles per word)

## Development Conventions

### Code Style

- All code is inline within the single HTML file (no external `.css` or `.js` files)
- CSS comments are in Korean (e.g., `/* 배경 애니메이션 */`, `/* 게임 영역 */`)
- JavaScript has no comments — logic is expressed through descriptive function names
- DOM elements are accessed via `document.getElementById()` cached in top-level `const` variables
- Game timing uses `setInterval` / `setTimeout` with `Date.now()` for elapsed-time checks

### Korean Language Context

- UI text is in Korean (instructions, labels, placeholder text)
- The word pool is entirely Korean vocabulary
- Font choices (Black Han Sans, Noto Sans KR) support Korean rendering
- The `<html lang="ko">` attribute is set

### When Making Changes

- **Keep it single-file.** Do not split into separate CSS/JS files unless explicitly requested.
- **Preserve the neon visual theme.** Use the existing CSS custom property colors.
- **Maintain Korean UI.** All user-facing text should remain in Korean.
- **No build step.** Changes should work by simply refreshing the browser.
- **Test by opening the file in a browser.** There is no automated test suite.

## Known Limitations

- No mobile/touch input support
- No accessibility attributes (ARIA labels, screen reader support)
- No error handling in JavaScript
- Word list is hardcoded (not externalized or configurable)
- No persistent high score storage
- Particle color selection has a logic quirk (re-rolls `Math.random()` multiple times in the ternary chain)
