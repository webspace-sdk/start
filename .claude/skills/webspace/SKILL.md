---
name: webspace
description: Create or change a webspace, a plain-HTML 3D multiplayer world rendered by the Webspace Engine. Use when asked to build, decorate, script, or reshape a world in this repository (index.html or other .html worlds), add objects, models, splats, text, or interactivity, or make a new world page.
---

# Building webspaces

A webspace world is one `.html` file: `<meta>` tags describe the environment, direct children of `<body>` are
objects placed with CSS transforms, and an optional `<script type="module">` brings it to life through the DOM.

## Before you start

1. Fetch and read https://webspaces.space/llms.txt (the full reference). Follow AGENTS.md in this repo.
2. Read the world file you're changing, top to bottom.

## Workflow

1. **Plan the place.** In a sentence or two: what it is, the mood (palette + terrain), what a visitor sees on
   arrival, and what people can do together. Prefer a few well-placed, meaningful objects over clutter.
2. **Environment.** Set `webspace.environment.terrain.type` (flat | plains | hills | islands) and all eight
   colors. Dark sky colors make night. Use `webspace.environment.fog = off` and `wrap = off` for large scenes
   or far-away objects.
3. **Objects.** Every body child gets a readable `id` and an explicit `transform` (cm, y up, facing −z from spawn).
   - Text: `<label>` (fits its content) or `<div>` (page) with `color`, `background-color`, `font-family`
     (serif, sans-serif, monospaced, fantasy, ui-rounded, cursive).
   - Emoji objects: `<div style="font-family: emoji; transform: ...">🌸</div>`.
   - Models: write small `.svox` files (Smooth Voxels: lines are z slices, space-separated groups are y layers
     bottom→top, characters are x voxels; letters map to material colors; `-` is empty). Keep them under ~20³
     and let `deform`/`lighting = smooth` do the shaping.
   - Real captures: `<model src="scan.spz">`; flip y-down captures with `rotateX(3.14159rad)`.
   - Glowing light: splats with `style="mix-blend-mode: plus-lighter"`.
4. **Behavior.** In a module script: `await webspace.ready;` then use DOM changes + events. Anything a visitor
   causes goes through `webspace.state.set(key, value)` and is applied in a `change` listener, so all visitors
   see it. Use `Date.now()` for motion that should look the same for everyone.
5. **Verify.** Serve the folder (`npx serve .`), open in Chrome, wait for load, look around, and check the console.
   Fix anything that errors or sits underground (ground height varies on non-flat terrain).

## Don't

- Don't add build tools, frameworks, or wrapper elements; don't put objects inside other objects.
- Don't rely on objects without transforms, or on ids for positioning.
- Don't delete `webspace.service.1.0.1.js`.
