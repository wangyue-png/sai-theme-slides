---
name: sai-master-slides-templates
description: Use when the user wants to create, customize, package, or export Sai-branded editable HTML slide decks using the Sai master slide template system. Supports minimal editorial Sai covers, editable text, slide templates, image/video media library, brand ASCII art assets with configurable colors, background images, layer controls, text motion, slide animation, and exportable single-file browser presentations.
---

# Sai Master Slides Templates

Use this skill to create or modify Sai-branded editable HTML slide decks. The skill is compatible with Codex-style and Claude-style skill loaders because it uses a standard `SKILL.md` file with YAML frontmatter and keeps reusable slide assets inside the same folder.

## Compatibility

- Codex: install or keep this folder where Codex can discover skills, then use the normal skill trigger.
- Claude: copy this entire folder into Claude's skills directory, preserving `SKILL.md`, `template/index.html`, and `template/assets/`.
- Both agents should treat `template/` as the reusable source template and should write generated decks to a separate output folder instead of editing the source template in place.
- If an environment cannot open a browser automatically, still create or edit the HTML and provide the absolute path to `template/index.html`.

## Core Workflow

1. Copy `template/` to the user's requested output folder.
2. Rename `template/index.html` if the user wants a custom filename.
3. Keep the `assets/` folder next to the HTML file so fonts, icons, brand images, and media backgrounds resolve.
4. Open the HTML directly in a browser, or give the user the absolute path to open.
5. For changes, edit `template/index.html` and verify the script parses.
6. Render a screenshot after meaningful visual changes and inspect spacing, alignment, and asset loading before finishing.

## Built-In Editor

The template includes:

- Direct text editing with the `Edit text` control
- Prioritized toolbar: top-level controls for primary actions, with secondary text styling grouped under the `Text` dropdown
- Rich text controls under `Text`: bold, italic, font family, and `A-` / `A+` font-size stepping by 1px with no upper limit
- Add-slide templates for title, cards, proof, dark close, workspace log, ASCII brand, desktop UI, comparison, quote, and metrics
- Image/video insertion and replacement
- ASCII generator controls for converting an uploaded image into a colored ASCII SVG asset
- Media uploads place the first uploaded asset onto the active slide immediately
- Drag-and-drop from the media library onto the active slide
- Image options for layering, fit/fill, background placement, and deletion
- Media sidebar with bundled backgrounds, drag/drop upload, and checkerboard thumbnails for transparent/light assets
- Text motion controls
- Slide animation controls
- Save/export controls
- History shortcuts, documented but not shown as toolbar buttons: `Ctrl/Cmd+Z`, `Ctrl/Cmd+Y`, and `Ctrl/Cmd+Shift+Z`
- Persistence shortcut: `Ctrl/Cmd+S` saves the complete `.slides-offset` deck state to `localStorage`

## Brand Guidance

- Product name is `Sai`.
- Use Adamina for main headings (`h1`, `h2`) and Manrope for body copy, UI controls, labels, and slide support text.
- Use Simular lime `#16D342` as the single strong accent.
- Keep product UI panels dense, restrained, and mostly monochrome.
- Prefer real Sai/Simular assets from `template/assets/` over generic decorative graphics.
- Current tagline: `Free from your computer`.
- Current one-liner: `Your 24/7 computer worker`.
- Bundle both heading and body fonts in `template/assets/`: `Adamina-Regular.ttf` and `Manrope-VariableFont_wght.ttf`.

## Cover Direction

Default Sai cover pages should be minimal and editorial.

- Do not use a classic split layout with headline on one side and a framed workspace/mockup on the other.
- Do not put a workspace UI, browser chrome, task log, cards, or product panels on the cover unless the user explicitly asks for a product-demo cover.
- Use the Sai mark, a small kicker, the tagline, the one-liner, and one subtle brand texture at most.
- Prefer a light editorial background such as `#F7F8F1` with black text and Sai lime used sparingly.
- A faint ASCII art watermark is allowed, preferably on the right side or cropped into a corner at very low opacity.
- All cover text should share a clear left edge. The Sai mark should align to the same content margin when possible.
- Keep generous breathing room around the headline. Avoid line-height that makes the second line crowd the one-liner.
- Adamina needs more vertical air than Manrope; use normal letter spacing and a safer line-height around `1.04-1.1` for cover/title headlines.
- The one-liner should sit visibly below the headline with enough vertical space to read as supporting copy, not a continuation of the title.
- Avoid extra descriptor text on the cover unless specifically requested.

## Typography And Spacing QA

Before delivering a cover or title slide:

- Check that kicker, headline, and one-liner are optically left-aligned.
- Check that no title descenders or oversized letters visually collide with the one-liner.
- Check that the text block is not pushed into browser/editor controls in the rendered preview.
- Check that brand art is subtle enough to support the text, not compete with it.
- Check that mobile or smaller browser previews do not crop essential text.

## Verification

After editing the HTML, run a JavaScript syntax check:

```bash
node -e 'const fs=require("fs"); const html=fs.readFileSync("template/index.html","utf8"); [...html.matchAll(/<script>([\s\S]*?)<\/script>/g)].forEach(m=>new Function(m[1])); console.log("ok")'
```

Render a screenshot with headless Chrome when available:

```bash
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --headless --disable-gpu --virtual-time-budget=3000 --screenshot=preview-cover.png --window-size=1440,900 file://$PWD/template/index.html
```

If Chrome is unavailable, use any available browser screenshot tool. If no browser rendering is available, say that visual verification was not run and report the syntax check result.

## ASCII Art Assets

Sai brand decks can use ASCII art textures generated from source images. Prefer SVG for reusable assets because the character color can be edited cleanly.

The HTML editor includes this as a visible toolbar feature:

- Use `ASCII` to upload a source image.
- Use the adjacent color picker to choose the generated character color.
- Use `ASCII 120`, `ASCII 160`, or `ASCII 200` to choose detail level before upload.
- The generated SVG-style ASCII asset is inserted onto the current slide and added to the media library.
- ASCII output must use a transparent SVG background by default; only the characters should be colored.
- Assets in the media library can be dragged directly onto the slide canvas to place them at the drop position.

Use this workflow when the user wants to generate brand ASCII assets:

1. Start from a simple high-contrast source image, silhouette, logo-like shape, or mask.
2. Convert to grayscale.
3. Resize to a text-friendly width, usually `100-180` characters.
4. Map brightness to characters such as `@%#*+=-:. `.
5. Export as SVG with transparent background when the asset needs to be recolored.
6. Save generated assets into `template/assets/` or the output deck's adjacent `assets/` folder.

Example Python generator:

```python
from PIL import Image
import html

CHARS = "@%#*+=-:. "

def image_to_ascii_svg(
    input_path,
    output_path="ascii-art.svg",
    width_chars=140,
    fg="#16D342",
    bg="transparent",
    font_size=10,
    invert=False,
):
    img = Image.open(input_path).convert("L")
    w, h = img.size
    height_chars = int(width_chars * (h / w) * 0.45)
    img = img.resize((width_chars, height_chars))
    chars = CHARS[::-1] if invert else CHARS

    lines = []
    for y in range(height_chars):
        row = ""
        for x in range(width_chars):
            p = img.getpixel((x, y))
            row += chars[p * (len(chars) - 1) // 255]
        lines.append(row.rstrip())

    line_height = font_size * 1.05
    svg_w = width_chars * font_size * 0.62
    svg_h = height_chars * line_height
    bg_rect = "" if bg == "transparent" else f'<rect width="100%" height="100%" fill="{bg}"/>'
    tspans = "".join(
        f'<tspan x="0" y="{(i + 1) * line_height:.2f}">{html.escape(line)}</tspan>'
        for i, line in enumerate(lines)
    )

    svg = f'''<svg xmlns="http://www.w3.org/2000/svg" width="{svg_w:.0f}" height="{svg_h:.0f}" viewBox="0 0 {svg_w:.0f} {svg_h:.0f}">
{bg_rect}
<text fill="{fg}" font-family="Menlo, Monaco, Consolas, monospace" font-size="{font_size}" xml:space="preserve">{tspans}</text>
</svg>'''

    with open(output_path, "w", encoding="utf-8") as f:
        f.write(svg)
```

Recommended Sai colors:

- Lime: `#16D342`
- Deep green: `#0B8226`
- Ink: `#161819`
- Editorial light: `#F7F8F1`
- Soft gray: `#A7ADA5`

If the user asks for a GitHub-ready package, include `README.md`, `SKILL.md`, `.gitignore`, and the full `template/` folder.
