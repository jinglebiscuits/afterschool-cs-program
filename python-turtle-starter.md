### Python Turtle Starter: Routes & Rails Jr

Use Trinket or Replit with Python (Turtle enabled). Paste into main.py.

```python
import turtle

screen = turtle.Screen()
screen.setup(700, 500)
screen.title('Routes & Rails Jr (Turtle)')

pen = turtle.Turtle()
pen.hideturtle()
pen.speed(0)
pen.pensize(4)

cities = [
    { 'id': 'A', 'x': -200, 'y': 120 },
    { 'id': 'B', 'x': 0,    'y': 160 },
    { 'id': 'C', 'x': -100, 'y': -40 },
    { 'id': 'D', 'x': 160,  'y':  20 },
]

routes = [
    { 'id': 'A-B', 'a': 'A', 'b': 'B', 'length': 2, 'owner': None },
    { 'id': 'B-C', 'a': 'B', 'b': 'C', 'length': 2, 'owner': None },
    { 'id': 'A-C', 'a': 'A', 'b': 'C', 'length': 3, 'owner': None },
    { 'id': 'C-D', 'a': 'C', 'b': 'D', 'length': 2, 'owner': None },
]

players = [
    { 'id': 0, 'name': 'Red',  'color': 'red',  'score': 0 },
    { 'id': 1, 'name': 'Blue', 'color': 'blue', 'score': 0 },
]

current_player = 0
hovered = None

def get_city(cid):
    for c in cities:
        if c['id'] == cid:
            return c
    return None

def draw_text(x, y, text):
    pen.up(); pen.goto(x, y); pen.down()
    pen.write(text, align='left', font=('Arial', 12, 'normal'))

def draw_circle(x, y, r, color='black'):
    pen.up(); pen.goto(x, y - r); pen.setheading(0); pen.down()
    pen.pencolor(color)
    pen.circle(r)

def draw_line(x1, y1, x2, y2, color='black', width=4):
    pen.pencolor(color)
    pen.pensize(width)
    pen.up(); pen.goto(x1, y1); pen.down()
    pen.goto(x2, y2)

def clear_screen():
    pen.clear()

def draw_board():
    clear_screen()
    # routes
    for r in routes:
        a = get_city(r['a']); b = get_city(r['b'])
        color = 'grey' if r['owner'] is None else players[r['owner']]['color']
        draw_line(a['x'], a['y'], b['x'], b['y'], color=color, width=6 if r == hovered else 4)
    # cities
    for c in cities:
        draw_circle(c['x'], c['y'], 7, color='black')
        draw_text(c['x'] - 5, c['y'] - 22, c['id'])
    # HUD
    p = players[current_player]
    draw_text(-340, 210, f"Turn: {p['name']} | Scores - Red: {players[0]['score']} Blue: {players[1]['score']}")
    draw_text(220, 210, 'Click near a route midpoint to claim')

def distance(x1, y1, x2, y2):
    return ((x2 - x1)**2 + (y2 - y1)**2) ** 0.5

def route_midpoint(r):
    a = get_city(r['a']); b = get_city(r['b'])
    return ((a['x'] + b['x'])/2, (a['y'] + b['y'])/2)

def end_turn():
    global current_player
    current_player = (current_player + 1) % len(players)
    draw_board()

def on_click(x, y):
    global hovered
    hovered = None
    # find closest unowned route midpoint within threshold
    best = None; best_d = 9999
    for r in routes:
        if r['owner'] is not None:
            continue
        mx, my = route_midpoint(r)
        d = distance(x, y, mx, my)
        if d < best_d:
            best_d = d; best = r
    if best is not None and best_d < 25:
        best['owner'] = current_player
        players[current_player]['score'] += best['length']
        end_turn()
    else:
        draw_board()

def on_move(x, y):
    global hovered
    hovered = None
    for r in routes:
        mx, my = route_midpoint(r)
        if distance(x, y, mx, my) < 25 and r['owner'] is None:
            hovered = r
            break
    draw_board()

draw_board()
screen.onclick(on_click)
screen.onscreenclick(on_click)
screen.cv.bind('<Motion>', lambda e: on_move(e.x - screen.window_width()/2, screen.window_height()/2 - e.y))
screen.mainloop()
```

Next steps
- Replace hardcoded data with lists/dicts loaded at top; add more cities.
- Add a simple "End Turn" button (use an invisible rectangle and click detection).
- Show claimed route color and running score.

