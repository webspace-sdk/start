# Working on this webspace (instructions for AI coding agents)

This repository is a **webspace**: HTML pages that the Webspace Engine renders as walkable, multiplayer 3D worlds.
Read **https://webspaces.space/llms.txt** before making changes. It's the complete authoring reference.

## Ground rules

- **The HTML file is the world.** Direct children of `<body>` are objects. Don't add wrapper divs, frameworks, or
  build steps. Keep worlds as plain static files.
- **Place everything with an explicit transform**: `style="transform: translate3d(Xcm, Ycm, Zcm) ..."`. Units are
  centimeters, y is up, and the default view looks toward −z. Objects without a transform spawn in front of each
  visitor separately, so don't rely on that.
- **Ground height varies** on `hills`, `plains`, and `islands` terrain (often 1–2 m above y = 0 near spawn). Use
  `flat` terrain when you need predictable heights, or place things ~150 cm above where you expect the ground.
- **Give objects readable ids** (`id="lantern-3"`) so scripts can find them with `getElementById`.
- **Scripts go in `<script type="module">`** and start with `await webspace.ready;`. Change the world by changing
  the DOM (`style.transform`, `hidden`, `src`, `innerHTML` of text). React with `click`, `pointerenter`, and
  `pointerleave` listeners. Use `webspace.state` (shared, last-writer-wins) for anything visitors cause, so
  everyone sees the same thing. Time-based motion from `Date.now()` is already in sync for everyone.
- **Script changes are never saved into the file**, so it's safe to animate every frame.
- **Models**: `.svox` (Smooth Voxels, plain text, the house style), `.glb`, and Gaussian splats (`.spz`, `.ply`,
  `.splat`). Keep assets in folders next to the HTML and use relative paths.
- **Keep `webspace.service.1.0.1.js`** next to every world's HTML. It's required when hosted.
- **Multiple worlds**: add more `.html` files and link them with `<a href>`. A `<nav><ul><li><a>` list in
  `index.html` becomes the site menu.

## Taste

Aim for places, not demos. Choose a deliberate palette (sky, ground, grass, leaves…), give visitors a sense of
arrival at spawn, place a few meaningful things with care, write text that speaks to the visitor, and give people
something to do together. The house style is soft, cel-shaded, and timeless.

## Checking your work

Serve the folder over HTTP (e.g. `npx serve .` or `python -m http.server`) and open it in Chrome. The world takes
several seconds to load. The browser console shows script errors. `window.webspace.state.toJSON()` shows shared
state.
