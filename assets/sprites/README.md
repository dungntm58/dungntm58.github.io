# Sprite assets

Drop PNG sprites here to replace the procedural placeholders. `PreloadScene`
loads each file by the path below; if a file is missing it falls back to the
old hand-coded pixel graphics, so the game keeps running either way.

Texture keys and paths are defined in `src/game/config/assets.ts`
(`ASSET_MANIFEST`). To add a new sprite, add an entry there and a fallback
draw function, then drop the PNG.

## Expected files

| File            | Key         | Size (px) | Used by                          |
| --------------- | ----------- | --------- | -------------------------------- |
| `player.png`    | `player`    | 32 × 32   | Player entity (both scenes)      |
| `librarian.png` | `librarian` | 32 × 32   | Librarian NPC (LibraryScene)     |
| `book.png`      | `book`      | 16 × 20   | Selectable books (LibraryScene)  |
| `torch.png`     | `torch`     | 8 × 40    | Reading-desk torch (LibraryScene)|
| `car.png`       | `car`       | 64 × 32   | Street cars (OutsideScene)       |

Notes:
- `player` / `librarian` are forced to 32×32 on screen via `setDisplaySize`, so
  higher-res PNGs scale down cleanly.
- `book` / `torch` / `car` render at native PNG size — match the sizes above.
- Keep transparent backgrounds (PNG alpha). The game sets
  `imageRendering: pixelated`, so crisp pixel art stays crisp.

## Generating art

Use the installed `pixel-art-sprites` skill, e.g.:

> Generate a 32×32 pixel-art sprite of a librarian NPC: silver hair, glasses,
> brown tunic, transparent background. Save to
> `public/assets/sprites/librarian.png`.

For animated sprites later, switch `this.load.image` to
`this.load.spritesheet(key, path, { frameWidth, frameHeight })` in
`PreloadScene` and define animations in the consuming scene.
