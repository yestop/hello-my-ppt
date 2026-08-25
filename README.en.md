# hello-my-ppt

A local PPTD-to-editable-PPTX skill. It provides a local browser editor, a local OOXML/PPTX writer, and a local page-image renderer. PPTX export does not depend on Kimi login, Kimi download endpoints, or a remote conversion service.

## Features

- Create and edit PPTD v2 projects: `deck.pptd` + `pages/` + `media/`
- Preview, live-reload, edit, and save PPTD in Chrome or Edge
- Generate editable `.pptx` files with the local writer
- Render page PNGs through the same local rendering pipeline
- Package the complete project as a ZIP
- Support text, shapes, tables, 11 chart types, formulas, SVG/PNG/JPEG/GIF, icons, and optional font embedding

## Quick start

Node.js 18+ is required. No `npm install` is needed.

```powershell
cd C:\path\to\hello-my-ppt

# Start the local browser editor
node bin/hello-my-ppt.js serve --project C:\path\to\my-deck

# Generate an editable PPTX locally, without Kimi
node bin/hello-my-ppt.js export C:\path\to\my-deck\deck.pptd `
  -o C:\path\to\my-deck\deck.pptx

# Render page images locally
node bin/hello-my-ppt.js render C:\path\to\my-deck\deck.pptd `
  -o C:\path\to\my-deck\rendered

# Package the complete PPTD project
node bin/hello-my-ppt.js export-project C:\path\to\my-deck\deck.pptd `
  -o C:\path\to\my-deck\deck-project.zip
```

The editor prints a local URL such as `http://127.0.0.1:55173/`. Open it in Chrome or Edge. Use `serve` for editing and preview, `export` for PPTX, and `render` for page images.

## Recommended project layout

```text
my-deck/
├── deck.pptd
├── pages/
│   ├── 01.page
│   └── 02.page
└── media/
    └── robot.png
```

All page and media paths must be relative paths inside the directory containing `deck.pptd`.

## Verify a PPTX

```powershell
node tests/package-integrity.mjs C:\path\to\my-deck\deck.pptx 24
```

For high-risk deliveries, open the PPTX in PowerPoint or WPS and inspect representative pages.

## Limits

- Existing PPTX files are not imported back into PPTD; rebuild them from reference material.
- Heatmap and Sankey are not exported as native PowerPoint charts.
- Unknown icons are skipped. Prefer `bs:<name>` icons from `references/icons.md`, or use a local SVG/PNG image.
- Original PPTX master structures are not preserved; recreate the visual system with PPTD theme tokens and native page elements.

## License and attribution

This distribution is based on the local PPTD/PPTX writer and editor architecture from `open-pptd`, with the `hello-my-ppt` name, localized documentation, and compatibility icon aliases added. See `NOTICE.md` in this repository.
