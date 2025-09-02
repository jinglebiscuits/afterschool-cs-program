### p5.js Starter: Routes & Rails Jr (simplified Ticket to Ride)

In your OpenProcessing Class (join link in `local/openprocessing-join-url.txt`), create a new sketch and paste the following into `sketch.js`. If OpenProcessing is unavailable, you can also use the p5.js Web Editor (`https://editor.p5js.org/`).

```javascript
// Minimal starter to draw a small map, click cities, and claim routes.

let cities = [
  { id: 'A', x: 120, y: 120 },
  { id: 'B', x: 320, y: 160 },
  { id: 'C', x: 220, y: 300 },
  { id: 'D', x: 420, y: 260 }
];

let routes = [
  { id: 'A-B', a: 'A', b: 'B', length: 2, owner: null },
  { id: 'B-C', a: 'B', b: 'C', length: 2, owner: null },
  { id: 'A-C', a: 'A', b: 'C', length: 3, owner: null },
  { id: 'C-D', a: 'C', b: 'D', length: 2, owner: null }
];

let players = [
  { id: 0, name: 'Red', color: '#e74c3c', score: 0 },
  { id: 1, name: 'Blue', color: '#3498db', score: 0 }
];

let currentPlayer = 0; // index into players
let hoveredRoute = null;
let message = '';

function setup() {
  createCanvas(600, 400);
  textFont('sans-serif');
}

function draw() {
  background(245);
  drawBoard();
  drawHud();
}

function drawBoard() {
  hoveredRoute = null;
  // Draw routes first
  for (let r of routes) {
    const a = getCity(r.a);
    const b = getCity(r.b);
    const midX = (a.x + b.x) / 2;
    const midY = (a.y + b.y) / 2;

    // Hover detection
    const d = dist(mouseX, mouseY, midX, midY);
    const isHover = d < 20;
    if (isHover && !r.owner) hoveredRoute = r;

    strokeWeight(isHover ? 6 : 4);
    if (r.owner != null) {
      stroke(players[r.owner].color);
    } else {
      stroke(180);
    }
    line(a.x, a.y, b.x, b.y);
  }

  // Draw cities
  noStroke();
  for (let c of cities) {
    fill(50);
    circle(c.x, c.y, 14);
    fill(20);
    textAlign(CENTER, TOP);
    text(c.id, c.x, c.y + 10);
  }
}

function drawHud() {
  noStroke();
  fill(0);
  textAlign(LEFT, TOP);
  textSize(14);
  const p = players[currentPlayer];
  text(`Turn: ${p.name}  |  Scores - Red: ${players[0].score}  Blue: ${players[1].score}` , 10, 10);
  if (message) {
    fill(60);
    text(message, 10, 30);
  }

  // Button: End Turn
  drawButton(width - 120, 10, 110, 28, 'End Turn', () => {
    endTurn();
  });
}

function mousePressed() {
  if (hoveredRoute && hoveredRoute.owner == null) {
    // Simple claim rule: anyone can claim an unowned route
    hoveredRoute.owner = currentPlayer;
    players[currentPlayer].score += hoveredRoute.length; // score by length
    message = `${players[currentPlayer].name} claimed ${hoveredRoute.id}`;
  }
}

function endTurn() {
  currentPlayer = (currentPlayer + 1) % players.length;
  message = '';
}

function getCity(id) {
  return cities.find(c => c.id === id);
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

  if (hover && mouseIsPressed) {
    onClick();
  }
}
```

Next steps (Weeks 4–7)
- Replace hardcoded cities/routes with arrays loaded from a JSON‑like object.
- Add per‑turn actions: draw card vs claim route.
- Create a tiny deck of colored cards and a hand UI.
- Implement basic end conditions and a score screen.

