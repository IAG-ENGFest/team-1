# Air Traffic Rush: Game Design Plan

> **Prompt:**
> You are GitHub Copilot helping me build a small browser game for a hackathon.
> Generate a complete, front-end-only web game in a single file named `indexc.html`. All logic and assets must run in the browser only (HTML, CSS, and vanilla JavaScript – no frameworks, no build tools, no backend, no external databases). The game will be hosted on GitHub Pages, so everything must work with static hosting and relative paths.

---

## Game Concept

Create a simple air-traffic-control game inspired by classic “Flight Control” style games.

### High-level Idea
- The player is an air traffic controller looking at a top-down map of an airport.
- Airplanes spawn at the edges of the screen and fly toward the airport.
- The player must assign each plane a landing path to a runway by clicking (or tapping) on the plane and drawing a path with the mouse.
- Each plane then follows its assigned path (a series of waypoints) and tries to land safely.
- If two planes get too close to each other (collision radius), the game is over.
- Every successful landing gives score points and contributes to level progression.

---

## Core Gameplay Rules

### 1. Spawning Planes
- Planes appear at random intervals at the edges of the canvas (top, bottom, left, or right) with a random heading toward the center.
- Each plane has:
  - A position (x, y)
  - A speed
  - A direction/heading
  - A color or small sprite that indicates its type
- For simplicity, use simple shapes (e.g., triangles or arrows) drawn on `<canvas>`. If you create image assets, store them under `./assets/` and reference them via relative paths.

### 2. Drawing Paths
- When the player clicks on a plane, enter a “path drawing” mode.
- While the mouse is held down and moved, show a polyline that follows the cursor.
- On mouse release, convert this drawn line into a list of waypoints for that plane.
- The plane will then steer and move along this path smoothly, heading from one waypoint to the next.
- If the player clicks a plane again, they can reassign a new path (overwrite the old one).

### 3. Runways and Landing
- Draw at least one runway on the map (a rectangular area).
- A plane is considered safely landed when:
  - It has a path that leads across the runway, and
  - Its position enters a small landing zone around the runway endpoint.
- When a plane lands:
  - Remove the plane from the active list.
  - Increase the score.
  - Optionally play a simple visual effect (e.g., brief highlight of the runway or a small text “Landed +1”).

### 4. Collisions and Game Over
- Continually check distances between all pairs of planes.
- If two planes come within a certain collision radius, trigger a collision:
  - Show a clear “Game Over” message.
  - Stop the game loop.
  - Allow the player to restart via a Restart button or pressing Enter.
- If a plane goes off-screen without landing, it should also count as a collision (game over).
- If a plane lands on the runway in the wrong direction (e.g., opposite to the runway orientation), it counts as a collision.

### 5. Levels and Difficulty
- Start at Level 1 with:
  - Slow planes (e.g., 1 pixel per frame)
  - One plane spawning at a time (every 3-5 seconds)
  - Planes appearing from the North
  - One horizontal runway in the centre of the screen
- Increase the level automatically every time the player reaches a certain score threshold (every 5 safe landings).
- Each new level should:
  - Slightly increase the spawn rate (shorter time between spawns).
- Every 1 levels:
  - Slightly increase plane speed (e.g., +0.2 pixels per frame)
- Every 2 levels:
  - Introduce one more runway (with a required landing direction)
- Every 3 levels:
  - Change required direction of landing on the runway
- Display the current Level in the HUD.
- At level 2 spawn planes from the South
- At level 3 spawn planes from the East
- At level 4 spawn planes from the West

### 6. Lives or Strikes (Optional)
- As a simple version, one collision = game over.
- If it’s easy, add strikes: e.g., player has 3 strikes, losing one on each collision or critical event.

---

## Controls and Interaction

- **Mouse / Trackpad:**
  - Click a plane to start drawing a path.
  - Drag to define the path.
  - Release to finish the path.
- **Keyboard:**
  - Press Enter to start the game if it’s not running, or to restart after Game Over.
- **Touch:**
  - Basic tap & drag behavior is enough if possible.

---

## UI, HUD, and Feedback

- Use a clean, modern, minimalistic UI.
- Show a title at the top, like “Air Traffic Rush” or “Mini Flight Control”.
- Include a HUD above or below the game canvas with:
  - Score (total successful landings)
  - Level
  - Optional: Strikes/Lives
- Add a Start / Restart button under the canvas.
- Add short instructions text:
  > “Click a plane and draw its route to the runway. Avoid collisions!”
- Show overlay messages:
  - “Level X” when the player levels up.
  - “Game Over – Score: Y – Press Enter or click Restart” when the game ends.
- Show required landing direction on the runway if applicable (e.g., arrows).
- Ensure everything is visible in the window without having to scroll.
- Ensure the play is widescreen-friendly (16:9 ratio)

---

## Technical Implementation Details

- Use a `<canvas>` element for the game area.
- Implement a game loop using `requestAnimationFrame`:
  - Update plane positions and check collisions.
  - Spawn new planes based on a timer and level.
  - Render background, runways, paths, and planes.
- Use plain ES6 JavaScript in a `<script>` tag inside the same `indexc.html`.
- All CSS should be in a `<style>` block within `indexc.html` as well.
- Make the canvas responsive-ish:
  - For desktop, keep a fixed ratio (e.g., 480x720 or similar) and center the canvas.
  - On small screens, scale down but preserve the aspect ratio.

---

## Visual Design

- **Background:**
  - Suggest a simple stylized airport map:
    - A soft gradient sky or ground.
    - Runways in darker grey with white stripes.
- **Planes:**
  - Simple shapes (triangles/arrows) with different colors to represent types.
- **Paths:**
  - Draw paths as thin lines in a bright color (e.g., white or yellow).
- Use CSS to give the page a polished feel: rounded corners, subtle shadows, and legible fonts.

---

## Structure of the HTML File

In the generated `indexc.html`, include:

1. **A `<head>` with:**
   - `<meta charset="UTF-8">`
   - Responsive viewport meta
   - A descriptive `<title>Air Traffic Rush</title>`
   - Embedded `<style>` for all layout and visual design.
2. **A `<body>` with:**
   - Page title and short description/instructions.
   - A HUD area that shows score, level, lives/strikes.
   - A container div holding the game `<canvas>` and an overlay message element.
   - A “Start / Restart” button.
3. **A `<script>` block at the bottom with well-structured JavaScript:**
   - Constants and configuration (speeds, radii, spawn intervals).
   - Data structures for planes, runways, and paths.
   - Input handling (mouse/touch + keyboard).
   - Update and render functions.
   - Collision detection.
   - Level progression and scoring logic.
   - Game state management (running, paused, game over).

---

## Copilot Guidance and Comments

- Before each major block of code, write a clear, descriptive comment explaining the purpose, for example:
  - `// GAME STATE: keeps track of active planes, score, level, and lives`
  - `// INPUT HANDLING: mouse events for selecting a plane and drawing a path`
  - `// GAME LOOP: updates positions, checks collisions, and renders each frame`
- Keep function names and variable names self-descriptive so the intent of the game logic is clear.
- At the end of the file, include a brief comment section like:
  - `// TODO ideas: add different plane types, sound effects, and a high-score system.`

---

## Testing and Polish

- Make sure the game starts correctly, planes spawn, and the player can:
  - Select a plane
  - Draw a path to the runway
  - Land planes and earn points
  - Trigger a game over when planes collide
  - Restart easily
- Avoid use of any external libraries or modules; this must run as a single static HTML file opened directly in a browser or via GitHub Pages.

---

*End of plan.*
