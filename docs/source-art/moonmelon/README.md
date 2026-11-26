# Moonmelon vector source masters

These are original, hand-authored SVG source masters for the Expanded Edition Moonmelon crop study.

- `moonmelon-stage1.svg`: young crop silhouette.
- `moonmelon-stage2.svg`: mature crop with visible ripe fruit.
- `moonmelon-harvest-icon.svg`: standalone produce icon.

All three are transparent-background vector masters. They are not yet runtime assets: the crop loader expects raster files at its established paths, and the exact in-game canvas, pivot, soil treatment, and icon dimensions must be checked against neighboring assets before export. No original commercial-game art is included in these files.

Suggested next art-pipeline step: compare a few existing crop PNGs in a local checkout, record their canvas and alpha conventions, then add a reproducible export step that preserves these SVGs as the source of truth.