---
name: sai-master-slides-templates
description: Use when the user wants to create, customize, package, or export Sai-branded editable HTML slide decks using the Sai master slide template system. Supports editable text, slide templates, image/video media library, background images, layer controls, text motion, slide animation, and exportable single-file browser presentations.
---

# Sai Master Slides Templates

Use this skill to create or modify Sai-branded editable HTML slide decks.

## Core Workflow

1. Copy `template/` to the user's requested output folder.
2. Rename `template/index.html` if the user wants a custom filename.
3. Keep the `assets/` folder next to the HTML file so fonts, icons, brand images, and media backgrounds resolve.
4. Open the HTML directly in a browser, or give the user the path to open.
5. For changes, edit `template/index.html` and verify the script parses.

## Built-In Editor

The template includes:

- Direct text editing with the `Edit text` control
- Add-slide templates for title, cards, proof, dark close, workspace log, ASCII brand, desktop UI, comparison, quote, and metrics
- Image/video insertion and replacement
- Image options for layering, fit/fill, background placement, and deletion
- Media sidebar with bundled backgrounds and drag/drop upload
- Text motion controls
- Slide animation controls
- Save/export controls

## Brand Guidance

- Product name is `Sai`.
- Use Manrope for the deck UI and slide typography.
- Use Simular lime `#16D342` as the single strong accent.
- Keep product UI panels dense, restrained, and mostly monochrome.
- Prefer real Sai/Simular assets from `template/assets/` over generic decorative graphics.

## Verification

After editing the HTML, run a JavaScript syntax check:

```bash
node -e 'const fs=require("fs"); const html=fs.readFileSync("template/index.html","utf8"); [...html.matchAll(/<script>([\s\S]*?)<\/script>/g)].forEach(m=>new Function(m[1])); console.log("ok")'
```

If the user asks for a GitHub-ready package, include `README.md`, `SKILL.md`, `.gitignore`, and the full `template/` folder.
