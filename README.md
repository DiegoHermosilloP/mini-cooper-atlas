# Mini Cooper Atlas

An interactive 3D teardown of a 1964–67 Morris Mini Cooper S (1275), built as a single
self-contained HTML file. Pull the **Disassemble** slider and the car separates along its
own assembly order — body off subframes, subframes off running gear — until all
**2,561 pieces** are laid out in one catalogue wall, sorted by system.

![Assembled](docs/assembled.png)

## What it does

- **2,561 named pieces** across **15 systems** — body shell, subframes, engine, transmission,
  suspension, brakes, steering, wheels, cooling, fuel & exhaust, electrical, lighting,
  interior, glass & trim, fasteners.
- **Disassemble slider** — 0 % assembled → 50 % exploded radially by system → 100 % laid out
  as a catalogue wall, with the camera squaring up and framing itself automatically.
- **Per-system toggles** with live counts, plus All / Body / Mechanical filters.
- **Click any piece** for its system, material, parent assembly, envelope in millimetres and
  how many are fitted to the car. Hover gives a tooltip.
- **Search** (`/`), camera presets (¾ / front / side / rear / plan), auto-rotate, reset.
  `0` and `1` jump between assembled and catalogue; `←` `→` nudge the slider.
- Light and dark themes, both driven from the viewer's own setting.

## How the model is built

There is no scanned CAD. Every piece is generated in the browser at load time from the
published figures for the car: wheelbase 2036 mm, track 1230 mm, bore 70.6 × stroke 81.3 mm,
7.5 in front discs, 10 in wheels, 3054 mm overall length, 1346 mm overall height.

- **Body pressings are lofted surfaces.** Each panel — bonnet, front wings, door skins, rear
  quarters, roof, boot lid, nose and tail — is built from its own cross-sections and stitched
  into one indexed `BufferGeometry` with computed vertex normals, so it shades as a
  continuous surface. Adjacent panels share their boundary vertices, so there are no seams.
  Pillars, the roof gutter and the wheel-arch lips are profile sweeps along their paths.
- **Everything else is instanced.** The remaining ~2,400 pieces are bucketed by
  (geometry archetype × material) into `InstancedMesh` batches — a few dozen draw calls for
  thousands of parts, with per-instance matrices rewritten each frame while the slider moves.
- **The engine is a real A-series stack** — crank, four rods and pistons with their rings,
  camshaft, eight valves with springs and collets, a 44-link timing chain, twin SU HS2
  carburettors; the gearbox sits in the sump with its bearings modeled roller by roller.

## Running it

It is one file with no build step:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

The only external dependency is three.js r160, loaded from a CDN.

## A note on accuracy

Names follow workshop-manual usage, and the dimensions come from published figures for the
car. The reference codes shown on each part card (`MC-ENG-0417`) are this atlas's own index —
they are **not** factory part numbers. This is a reading of the car, not a scan of one.

## Licence

MIT. "Mini" and "Cooper" are trademarks of their respective owners; this is an unaffiliated
educational project.
