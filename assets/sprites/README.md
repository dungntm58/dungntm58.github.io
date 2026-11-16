# Sprite assets

Every sprite the game loads is listed in `src/game/config/assets.ts`
(`ASSET_MANIFEST`). `PreloadScene` loads them all and logs an error for any file
that is missing — there are no procedural placeholders.

`manifest-integrity.test.ts` fails if a manifest entry has no PNG, if a PNG on
disk is not in the manifest, or if an animation needs more frames than its sheet
declares.

## Style

- **Outside** — a modern street: flat-vector illustration, soft daylight.
- **Library** — a 14th-century monastic scriptorium: hand-painted, warm
  candlelight, isometric 2:1. Wall pieces (`oak_door`, `tapestry`,
  `gothic_window`) are mapped onto the left/right iso wall planes; `masonry` is a
  2:1 floor diamond.
- Portraits (`public/assets/portraits`) and the book frames
  (`public/assets/book`) share the same two registers.

## Authoring size

Sprites are authored at **2× their on-screen size** (`ART_SCALE = 0.5`), so the
GPU's bilinear sample is an exact box-filter downscale. Phaser runs with
`antialias: true` (no `pixelArt`); display a sprite with `.setScale(ART_SCALE)`
or a per-sprite factor on top of it.

Sheets are horizontal strips with a fixed frame size (declared in the manifest):

| Sheet | Frames | Notes |
| --- | --- | --- |
| `player_side` | 2 | 0 = long stride, 1 = legs together; faces left |
| `player_front` / `player_back` | 3 | 0 = standing, 1/2 = a foot lifted; walk plays 1,0,2,0 |
| `ped_a`, `ped_b`, `ped_c` | 2 | stride / standing; all face left |
| `librarian_walk` | 2 | 0 = long stride, 1 = standing, feet together; faces left, flipped for rightward travel |
| `librarian_read` | 2 | 0 = closed book, 1 = reading (rest pose); drawn facing SW, flipped to face SE |
| `librarian_station` | 2 | the same desk and stool WITHOUT the monk, registered to `librarian_read` |

Cars are single images; `wheel_<colour>.png` is a disc cut from that car's own
wheel, drawn over the baked wheel and rotated as it drives (geometry in
`CAR_WHEELS`). The bookcase sprite includes its gallery and spiral stair.

Flames are separate sprites (`flame_a`, `flame_b`) swapped on a shared timer in
`LibraryScene`, so candles, braziers and sconces are static bodies.

## How the art is made

Generated and cut out in **Affinity** through the `affinity-scripting` skill
(Canva AI `generateImage`, `removeBackground`, `generativeEditImage`), one master
document per scene (`outside.afdesign`, `library.afdesign`) with each sprite as a
named layer. Export transparent PNGs from there, trim any baked ground shadow,
and resize to 2× display size. There is no seed, so a generation cannot be
reproduced — treat the PNGs and the Affinity masters as the source of truth, and
edit the layer rather than regenerating when a sprite needs a touch-up.
