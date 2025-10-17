# Snake — Minimal, Modern, Playable

## Summary
This repository contains a minimal, production‑ready Snake game that runs entirely in the browser. It is dependency‑free, responsive, and works on both desktop and mobile devices. The UI is clean and modern, and the game plays smoothly using requestAnimationFrame with a fixed‑step update loop.

Bonus: You can optionally pass a configuration JSON via the url query parameter (?url=...) to customize the grid size, speed, and theme colors.

- No build step, no external assets
- Works out of the box on GitHub Pages
- Keyboard and swipe controls
- High‑DPI canvas scaling for crisp rendering
- LocalStorage best score

## Setup
There is no setup required.

- Option A: Open index.html in any modern browser.
- Option B: Deploy to GitHub Pages:
  1. Create a repository and add these files.
  2. Push to GitHub.
  3. Enable GitHub Pages (Settings → Pages → Source: Deploy from a branch → select main and root).
  4. Visit the provided Pages URL.

The game works immediately after loading.

## Usage
- Start: press Space or click/tap Start.
- Move: Arrow keys or WASD on desktop, swipe on mobile.
- Pause/Resume: press P or click/tap Pause.
- Restart: press R or click/tap Restart.

Best score is saved across sessions in your browser’s localStorage.

### Optional configuration via ?url=...
You can pass a public JSON file containing configuration to tweak gameplay and theme. Append a url query parameter named url to the page URL:

Example:
https://your-username.github.io/your-repo/?url=https://example.com/snake-config.json

The JSON schema (all fields optional):
{
  "rows": 20,                 // integer, 8..64
  "cols": 20,                 // integer, 8..64
  "speed": 8,                 // integer, 3..25 (moves per second)
  "accelerateOnEat": true,    // boolean
  "maxSpeed": 16,             // integer, 5..60
  "theme": {
    "bg": "#1b2030",          // hex color for board background
    "grid": "#182030",        // hex color for grid lines
    "snake": "#35d07f",       // hex color for snake
    "food": "#ff5a5f"         // hex color for food
  }
}

Notes:
- The config is fetched once on load. If the request fails (network/CORS/invalid JSON), the game falls back to defaults and shows a status message.
- Only valid and safe values are applied; everything else is ignored.
- Use HTTPS and ensure the server serving your JSON allows cross‑origin requests if hosted on a different origin.
- You can also serve a data URL if desired (make sure it’s valid JSON).

## Code Explanation
The entire application resides in index.html. It includes:
- HTML: Accessible structure with a header, status bar, canvas, and overlay for idle/pause/game‑over messages.
- CSS: A minimal, modern theme using CSS variables and subtle glassy panels. The canvas is responsive and high‑DPI aware.
- JavaScript:
  - Configuration: Defaults are defined in a config object. If a ?url=... parameter is present, the app fetches the JSON and applies validated settings (grid size, speed, acceleration, theme colors).
  - Game loop: Uses requestAnimationFrame with a fixed‑step accumulator for deterministic updates based on moves per second.
  - Grid/canvas: The board is drawn on a high‑DPI‑scaled canvas. The cell size is calculated to fit the chosen rows/cols; a subtle grid is drawn to aid visibility.
  - Snake mechanics: The snake is an array of segments. Each tick advances the head in the current direction, checks for collisions, and grows on food consumption. Reversing direction into yourself is prevented.
  - Controls: Keyboard input (arrows/WASD, Space/P/R) and basic swipe detection for mobile. Buttons provide Start/Pause/Restart actions.
  - UI/UX: A status bar shows messages, a score chip displays current and best scores, and a translucent overlay appears for idle, pause, and game‑over states.
  - Persistence: Best score is stored in localStorage (key: snake_best).

Key functions/components:
- loadConfigFromUrl(): Reads the url parameter and fetches JSON. Times out gracefully and validates inputs.
- applyRemoteConfig(): Sanitizes and applies config values and theme colors.
- createGame(): Encapsulates game state and exposes start/pause/resume/restart/reset and setDirection.
- draw(): Renders the board, grid, food, and snake each frame. Includes subtle head eyes on larger cells.
- Input handlers: Map keyboard codes to directions and detect swipes for mobile users.

Error handling and robustness:
- All external fetches are optional and wrapped in try/catch with a timeout.
- Invalid or out‑of‑range config values are ignored or clamped.
- The app is fully functional without any configuration parameter.

## License
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.