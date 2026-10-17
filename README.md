# Fantasy Map Editor

A browser-based editor for annotating fantasy and world maps with pins, regions, drawings, and linked lore. It is a single self-contained HTML file with no dependencies, no build step, and no server.

**[Try it live →](https://ashe-barone.github.io/fantasy-map-editor/)**

<!-- Add a screenshot: save it as docs/screenshot.png and uncomment the line below -->
<!-- ![Fantasy Map Editor](docs/screenshot.png) -->

## Features

- **Pins** with 69 vector icon shapes, categories, tags, groups, images, and rich-text descriptions (headings, callouts, tables, embedded images)
- **Wiki-style links**: `[[Pin Name]]` inside a description jumps to that pin
- **Regions**: freehand polygons, circles, stars, arcs, and rectangles, with draggable vertex editing
- **Freehand drawings** for roads, rivers, and borders, simplified automatically
- **Relationship graph**: a force-directed view of every pin and link
- **Fog of war**: paint and erase to reveal the map gradually
- **Multi-map projects**: world, region, and city maps in one project file
- **Stamps**: place or scatter-brush terrain symbols, or fill a region with them
- **Tours and saved scenes**: animated camera paths for presenting a map
- **Undo/redo**, autosave to local storage, find and replace, minimap, measuring tool, and square or hex grid
- **Exports**: full project JSON, a standalone read-only viewer (one HTML file with images embedded), an editable copy, a PNG, and printable pin cards or a pin index

## Running it

Open `index.html` in any modern browser. Nothing else is required.

To host it, enable GitHub Pages on this repository (Settings → Pages → deploy from the `main` branch, root folder).

## Technical notes

The editor is written in plain JavaScript, HTML, and CSS, with no framework or libraries.

| Area | Approach |
| --- | --- |
| Rendering | Layered DOM: `<img>` map layers, SVG overlays for regions, links, and stamps, and a `<canvas>` for fog. All layers share one CSS transform for pan and zoom. |
| Pin realignment | Computes a 2D similarity transform (rotation, uniform scale, translation) from two anchor-point correspondences, then applies it to every pin. |
| Freehand drawing | Ramer–Douglas–Peucker line simplification, with a tolerance that scales with stroke width and zoom level. |
| Region fill | Ray-casting point-in-polygon test to scatter stamps inside arbitrary shapes. |
| Relationship graph | Force-directed layout with pairwise repulsion, spring attraction along links, and centering gravity, run on a canvas loop until it settles. |
| Undo/redo | Bounded snapshot stacks (50 states) of pins, links, drawings, and regions. |
| Persistence | Versioned project JSON with a migration chain, so older save files still load. |
| Export viewer | Builds a standalone HTML document at runtime. Map images are embedded as data URLs, and pin icon definitions are serialized from the editor's own shape table so the viewer and editor always match. |

## Project file format

Projects save as JSON containing `pins`, `links`, `drawings`, `regions`, `stamps`, `categories`, `templates`, base64-encoded `layers`, and fog state. The `version` field drives `migrateProject()` when an older file is loaded.

## License

MIT. See [LICENSE](LICENSE).
