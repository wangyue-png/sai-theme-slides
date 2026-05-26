# Sai Master Slides Templates

Editable, browser-native master slide templates for Sai presentations.

This project packages a single-file HTML slide editor with Sai/Simular styling, editable text, slide templates, image/video import, a media sidebar, background image selection, slide animation, text motion, and export support.

## What's Included

- `template/index.html` - the editable slide deck/editor
- `template/assets/` - bundled fonts, Sai/Simular marks, icons, integrations, brand art, and local media backgrounds
- `SKILL.md` - Codex skill instructions for using this project as a reusable slide-generation skill

## Use Locally

Open this file in a browser:

```text
template/index.html
```

The editor runs fully in the browser. No build step is required.

## Editor Features

- Edit text directly in the deck
- Add slide templates
- Delete slides
- Import images and videos
- Drag, resize, replace, layer, fit, fill, delete, or set images as slide backgrounds
- Use the Media sidebar to choose bundled or uploaded background images/videos
- Apply text motion and slide animation
- Save edits to browser localStorage
- Export the edited HTML

## Install As A Codex Skill

Copy this folder into your Codex skills directory:

```bash
cp -R sai-master-slides-templates ~/.codex/skills/
```

Then restart or refresh Codex so the skill list reloads.

## Repo Structure

```text
sai-master-slides-templates/
├── README.md
├── SKILL.md
├── .gitignore
└── template/
    ├── index.html
    └── assets/
```

## Notes

Large user-uploaded videos can exceed browser localStorage limits. Bundled media files in `template/assets/media/` are referenced by path and are better for reusable background libraries.
