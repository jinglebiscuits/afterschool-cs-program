### p5.js Starter: Shared Core + Game or Art Track

In your OpenProcessing Class (join link in `local/openprocessing-join-url.txt`), create a new sketch and paste the following into `sketch.js`. If OpenProcessing is unavailable, use the p5.js Web Editor (`https://editor.p5js.org/`).

This starter provides a shared core: draw shapes, track state, handle mouse, and simple UI buttons. From Week 4 onward, students can choose a track:
- Game Track: build a small game of their choice (clicker, dodge, platform-lite, pong variant, etc.).
- Art Track: create generative art or interactive posters (patterns, randomness, symmetry, palettes).

```javascript
// Shared core: canvas, simple state, basic UI, and input hooks.

let app = {
  screen: 'home', // 'home' | 'play' | 'gallery'
  theme: {
    bg: 245,
    ink: 20,
    accent: '#2ecc71'
  },
  clicks: 0,
  t: 0 // time counter for animations
};

function setup() {
  createCanvas(640, 400);
  textFont('sans-serif');
}

function draw() {
  background(app.theme.bg);
  app.t += 1;

  if (app.screen === 'home') drawHome();
  else if (app.screen === 'play') drawPlay();
  else if (app.screen === 'gallery') drawGallery();

  drawHud();
}

function drawHome() {
  fill(app.theme.ink);
  textAlign(LEFT, TOP);
  textSize(20);
  text('Welcome! Choose a track:', 16, 16);

  // Buttons to switch modes. Students can relabel to their game/art title later.
  drawButton(16, 56, 160, 36, 'Start (Game Track)', () => app.screen = 'play');
  drawButton(16, 100, 160, 36, 'Start (Art Track)', () => app.screen = 'gallery');
}

// Game track placeholder: a simple clicker/minigame scaffold
let player = { x: 320, y: 200, r: 14, speed: 3 };
let target = { x: 120, y: 120, r: 10 };
let score = 0;

function drawPlay() {
  // Example mechanics students can replace: move with arrow keys, collect target
  handleArrowMovement();
  const d = dist(player.x, player.y, target.x, target.y);
  if (d < player.r + target.r) {
    score += 1;
    randomizeTarget();
  }

  // Draw target
  noStroke();
  fill('#e74c3c');
  circle(target.x, target.y, target.r * 2);

  // Draw player
  fill('#3498db');
  circle(player.x, player.y, player.r * 2);

  // Score text
  fill(app.theme.ink);
  textAlign(LEFT, TOP);
  textSize(14);
  text(`Score: ${score}`, 10, 10);
}

function handleArrowMovement() {
  if (keyIsDown(37)) player.x -= player.speed; // Left
  if (keyIsDown(39)) player.x += player.speed; // Right
  if (keyIsDown(38)) player.y -= player.speed; // Up
  if (keyIsDown(40)) player.y += player.speed; // Down
  player.x = constrain(player.x, player.r, width - player.r);
  player.y = constrain(player.y, player.r, height - player.r);
}

function randomizeTarget() {
  target.x = random(20, width - 20);
  target.y = random(60, height - 20);
}

// Art track placeholder: generative pattern that reacts to mouse
function drawGallery() {
  // Grid of circles whose size and color depend on time and mouseX/mouseY
  const cols = 16;
  const rows = 10;
  const cellW = width / cols;
  const cellH = (height - 50) / rows;
  noStroke();
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      const cx = i * cellW + cellW / 2;
      const cy = j * cellH + cellH / 2 + 50;
      const n = noise(i * 0.2, j * 0.2, app.t * 0.01);
      const size = map(n, 0, 1, 4, 28) + map(mouseX, 0, width, -6, 6);
      const hue = map(mouseY, 0, height, 180, 340);
      fill(color(`hsla(${hue}, 70%, ${map(n, 0, 1, 35, 65)}%, 0.9)`));
      circle(cx, cy, max(2, size));
    }
  }

  // Title
  fill(app.theme.ink);
  textAlign(CENTER, TOP);
  textSize(14);
  text('Move the mouse to remix the pattern. Click to shift palette.', width / 2, 8);
}

function mousePressed() {
  app.clicks += 1;
  // Palette shift for art track
  if (app.screen === 'gallery') {
    const palettes = ['#2ecc71', '#e67e22', '#9b59b6', '#1abc9c'];
    app.theme.accent = random(palettes);
  }
}

function drawHud() {
  // Simple top-right buttons
  const y = 8;
  drawButton(width - 108, y, 100, 28, 'Home', () => app.screen = 'home');
}

function drawButton(x, y, w, h, label, onClick) {
  const hover = mouseX >= x && mouseX <= x + w && mouseY >= y && mouseY <= y + h;
  fill(hover ? 230 : 240);
  stroke(180);
  rect(x, y, w, h, 6);
  noStroke();
  fill(20);
  textAlign(CENTER, CENTER);
  text(label, x + w / 2, y + h / 2);
  if (hover && mouseIsPressed) onClick();
}
```

Suggested progression (Weeks 1–7 shared, then diverge)
- Weeks 1–2: Unplugged algorithms, classroom norms, and drawing basics in p5.js.
- Week 3: Shared core — canvas, shapes, mouse, simple UI button.
- Week 4: Students pick a track; outline features or visual goals on paper.
- Weeks 5–7: Implement features iteratively (small commits):
  - Game: character movement, goals/score, obstacles, levels, sounds.
  - Art: randomness, repetition, palettes, symmetry, animation, save PNG.
- Weeks 8–9: Testing, feedback, accessibility tweaks, polish.
- Weeks 10–13: Add stretch goals; prepare showcase pieces and short demos.


