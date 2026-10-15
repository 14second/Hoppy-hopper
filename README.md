# Hoppy Hopper

A tiny Crossy-Road-style hopping game in a single self-contained HTML file (~4 KB, ~110 lines of JavaScript). No build step, no dependencies, no assets — just open it.

**Play it live:** https://14second.github.io/Hoppy-hopper/ (GitHub Pages, served from `main` branch root)

## How it plays
- **Goal:** hop forward as far as you can without getting hit by traffic.
- **Controls:** tap / click / `Space` / `ArrowUp` — each input hops the frog forward exactly one lane.
- **Scoring:** score = the furthest lane reached. It never decreases.
- **Death:** if the frog's lane is a road and a car overlaps the frog's x-position, the game shows "SPLAT!" — tap again to restart from lane 0.
- **World:** lanes are generated infinitely ahead of the player. ~65% of lanes are roads with 1–3 moving cars (random direction and speed); the rest are safe grass.

## Code structure (`index.html` — the only file)
| Piece | What it does | Where |
|---|---|---|
| `ROWS = 9`, `PLAYER_ROW = 2` | 9 lanes visible; frog sits 2 lanes up from the bottom (camera anchor) | top of `<script>` |
| `size()` | Sizes the canvas to the viewport; re-runs on resize | top of `<script>` |
| `makeLane(i)` | Procedurally builds one lane: road or grass, car count/positions, direction, speed | mid `<script>` |
| `ensure()` | Generates lanes ahead of the player so the world is infinite | mid `<script>` |
| `hop()` | Advances `playerLane` by 1, updates score; restarts if dead | mid `<script>` |
| `frame(t)` | The game loop (`requestAnimationFrame`): moves cars with delta-time, checks collisions, follows the frog with the camera, draws everything on canvas | bottom of `<script>` |
| Rendering | Canvas 2D: grass lanes alternate greens, roads are dark with yellow dashes, cars are rounded rectangles in red/blue/orange, frog is a green circle with eyes | inside `frame(t)` |

State lives in four variables: `lanes[]`, `playerLane`, `score`, `dead`. There is no framework, no game engine, no external library.

## How to change it (for AI agents)
The whole game is plain JavaScript in one `<script>` block. Common tweaks:
- **Difficulty:** in `makeLane`, `rnd() < 0.65` is the road probability; `1 + Math.floor(rnd() * 3)` is cars per road; `0.12 + rnd() * 0.30` is car speed. Raise any of these to make it harder.
- **Fairness / hitbox:** collision is `Math.abs(x - 0.5) < 0.15` in `frame(t)` — `0.5` is the frog's x-center, `0.15` is the hitbox half-width. Shrink it for a friendlier game.
- **Camera / view:** `PLAYER_ROW = 2` controls how many lanes render below the frog; `ROWS = 9` is total visible lanes.
- **Look:** colors are hex literals in the render section (`#7ec86e` grass, `#3c3f46` road, `#f0c850` dashes, car colors array, `#4bb45a` frog).
- **New mechanics:** add variables next to `score`/`dead`, extend `hop()` for input, extend `frame(t)` for per-frame logic, and draw in the render section. Keep it dependency-free so GitHub Pages keeps working with zero config.
- **Testing a change:** open `index.html` directly in any browser — no server needed. The file must stay fully self-contained (no external URLs) for the Pages deploy to keep working.

## Deploy
Pushed to `main`; GitHub Pages is set to **Deploy from a branch → `main` → `/ (root)`**, so every push to `main` redeploys automatically within a minute or two.
