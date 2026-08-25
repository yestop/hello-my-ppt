---
name: hello-my-ppt
description: Create, edit, preview, render, and export editable PowerPoint presentations from local PPTD v2 projects. Use for PPT/PPTX, slide decks, PPTD, infographics, and posters when local browser editing, local PPTX generation, or page-image rendering is needed without Kimi or a remote export service.
---

# hello-my-ppt

`hello-my-ppt` is a local PPTD v2 presentation workflow. It includes a browser editor, a self-contained OOXML/PPTX writer, local page-image rendering, project packaging, charts, shapes, icons, formulas, images, and optional font embedding.

The export path is local. Do not route PPTX generation through Kimi, a login session, or a remote conversion API.

## Default deliverables

For presentation creation or editing, produce:

1. A self-contained project directory containing the `.pptd` manifest, `pages/`, and `media/` dependencies.
2. A locally generated editable `.pptx`.
3. Page PNGs only when the user requests images, visual QA, or a preview artifact.

## Prerequisites

- Node.js 18 or newer. The bundled runtime does not require `npm install`.
- Chrome or Edge only for the local browser editor and the local `render` command.
- PPTX export itself does not require Kimi, a Kimi account, or network access.

Check the runtime before starting:

```powershell
node --version
```

## Project contract

Use this layout:

```text
deck/
  deck.pptd
  pages/
    01.page
  media/
    image.png
```

The manifest and all referenced pages/media must stay inside the project directory. Paths are relative to the manifest. The supported format is PPTD v2; read `references/pptd.md` before designing or editing pages.

This skill does not import an existing PPTX into editable PPTD. For a user-provided PPTX, inspect it as a reference and rebuild the target content in a PPTD project. Preserve user-provided images in `media/` and use generated images only when they improve the communication goal.

## Workflow

### 1. Understand and plan

- Read the relevant files supplied by the user and `references/pptd.md`.
- Decide the page count, purpose, visual system, and content structure.
- Use a provided PPTD project as the template when one exists.
- Keep text, shapes, charts, tables, icons, and images as native PPTD elements whenever possible.

### 2. Start local preview early

Start the editor as soon as the manifest exists so the user can watch pages appear and edit them locally:

```powershell
node bin/hello-my-ppt.js serve --project C:\absolute\path\to\deck
```

Open the printed `http://127.0.0.1:<port>/` URL in Chrome or Edge. The editor supports live reload, local folder authorization, direct page editing, and saving changes back to the project.

### 3. Export editable PPTX locally

```powershell
node bin/hello-my-ppt.js export C:\absolute\path\to\deck\deck.pptd `
  -o C:\absolute\path\to\deck\deck.pptx
```

The writer creates native PowerPoint text, shapes, charts, tables, formulas, and SVG/image elements. Fade transitions are written by the local writer. Use `--no-embed-fonts` when system-font fallback is preferred.

### 4. Render page images locally

Render all pages:

```powershell
node bin/hello-my-ppt.js render C:\absolute\path\to\deck\deck.pptd `
  -o C:\absolute\path\to\deck\rendered
```

Render one page or use a larger scale:

```powershell
node bin/hello-my-ppt.js render deck.pptd -o rendered --page 3 --scale 2
```

Rendering uses the local editor pipeline and does not call Kimi. Review the PNGs for bounds, clipping, overflow, contrast, image distortion, and occlusion before final delivery when visual QA is requested.

### 5. Package the editable project

```powershell
node bin/hello-my-ppt.js export-project C:\absolute\path\to\deck\deck.pptd `
  -o C:\absolute\path\to\deck\deck-project.zip
```

### 6. Verify the PPTX

After export, confirm the output exists, the ZIP is readable, and the slide count matches:

```powershell
node tests/package-integrity.mjs C:\absolute\path\to\deck\deck.pptx <slide-count>
```

For higher-risk decks, also open the PPTX in PowerPoint or WPS and inspect representative pages. Do not claim Office compatibility from ZIP validation alone.

## Capability limits

- Heatmap and Sankey charts are not exported as native PowerPoint charts; avoid them or replace them with shapes.
- Unknown icons are skipped. Prefer `bs:<name>` icons listed in `references/icons.md`, or use a local SVG/PNG image for unsupported symbols.
- `fab:` brand icons are not bundled; use a licensed local image instead.
- Existing PPTX files are references, not directly importable PPTD sources.
- A custom PPTX master/template is not preserved by this local writer; recreate the desired visual system in PPTD elements and theme tokens.

## Local commands

```text
node bin/hello-my-ppt.js serve          # browser editor
node bin/hello-my-ppt.js export         # local PPTX writer
node bin/hello-my-ppt.js render         # local PNG renderer
node bin/hello-my-ppt.js export-project # self-contained project ZIP
node bin/hello-my-ppt.js fonts list     # bundled font registry
node bin/hello-my-ppt.js fonts check    # inspect deck font declarations
```

The original `bin/open-pptd.js` entry point remains as a compatibility alias inside this distribution; new workflows should use `bin/hello-my-ppt.js`.
