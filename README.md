# i hate designing — portfolio

A single-page portfolio hero: one retro computer on one wooden desk. Black
background, blank blue screen, thin purple line around the viewport.

No build step, no dependencies, no framework. Just open it.

## Run

```bash
node server.mjs
```

Then visit <http://localhost:5173>. (Opening `index.html` directly in a browser
works too — nothing here needs a server.)

## Files

```
index.html        the scene markup
styles/
  tokens.css      colors + the single scale unit (--u) + geometry
  base.css        reset, page shell, purple viewport frame, .sr-only
  scene.css       .desk / .computer (.monitor + .stand) / .keyboard
server.mjs        ~35-line static server, zero deps
```

## How it scales

Everything in the scene is sized as `calc(var(--u) * n)`, so the whole setup
scales as one object instead of as independent parts:

```css
--u: clamp(3.6px, min(1.45vw, 1.02vh), 8px);
```

`vw` keeps the monitor from outgrowing narrow screens; `vh` keeps the setup from
outgrowing short ones. Change `--u` and the entire scene resizes together.

Vertical composition is anchored, not guessed:

- `.desk` is `--desk-h` tall, pinned to the bottom.
- `.computer` is pinned at `bottom: var(--desk-h)`, so the stand base lands
  exactly on the desk's top edge.
- `.keyboard` sits `--keyboard-lift` up from the bottom, on the desk.
- The cord's height is derived from `--desk-h`, so it always reaches the stand.

## The desk

Wood is CSS gradients only — no image assets. Four stacked layers: plank seams
(dark groove + light lip), a tone shift on an off-period so planks don't look
cloned, then coarse and fine grain tilted `178deg` / `181deg` so no two grain
lines are parallel. `.desk::before` adds elliptical knot rings.

## Extending it: desktop icons

`.monitor__screen` is deliberately an empty container. Icons go inside it and
become the portfolio nav:

```html
<div class="monitor__screen">
  <ul class="desktop">
    <li><a class="icon" href="#work">…</a></li>
  </ul>
</div>
```

Size icons in `--u` like everything else so they scale with the monitor.

## Motion

One effect: the screen glow breathes over 7s. It lives inside
`@media (prefers-reduced-motion: no-preference)`, so reduced-motion users get
the same scene, held still.
