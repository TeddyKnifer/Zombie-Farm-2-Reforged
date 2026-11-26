# Expanded Edition — production art implementation spec

**Status: implementation brief only.** This document translates the approved-in-principle visual concepts into the repository's existing runtime data shapes. It does not add runtime assets, catalog entries, gameplay rules, save fields, or server behavior.

## Non-negotiable repository constraints

- Read `CONTRIBUTING.md` and `PROVENANCE.md` before production work.
- Do not extract or commit additional art/audio from the original commercial game. Every new file must be original or have a compatible, recorded licence.
- Do not commit generated atlas output when an existing `tools/prep_*.py` script should generate it. Commit the source art and the reproducible prep-script change instead.
- Runtime URLs must be rooted at `BASE` from `src/base.ts`, not hardcoded to `/assets`.
- Keep this first slice Local Farm only unless the maintainer explicitly approves a server-catalog route. No server-side price, reward, or raid changes in the initial art slice.
- Existing saves must load unchanged. Do not change persisted mutation bit order or introduce a save migration for a cosmetic/content-only addition.

## Crop: Moonmelon

The runtime crop contract is `PlantDef` in `src/assets.ts`:

| Field | Required production value |
|---|---|
| `key` | Stable lowercase identifier: `moonmelon` |
| `name` | Player-facing plural display name |
| `cost`, `sell`, `growMs`, `level`, `xp` | Balance values; deliberately unset until reviewed |
| `stage1` | Filename for the first plant sprite |
| `stage2` | Filename for the mature plant sprite |
| `icon` | Separate harvested-produce icon filename |

Runtime locations are already defined in `src/assets.ts`:
- `stage1` and `stage2` load from `public/assets/crops/`.
- `icon` loads from `public/assets/crop-icons/`.
- The seed stage is the shared seeded-soil texture; do not create a third crop-stage entry unless the engine contract changes.

**Art deliverables**
1. `moonmelon_stage1.png`: young vine with clear silhouette and transparent background where the existing crop art convention expects it.
2. `moonmelon_stage2.png`: mature plant with a clearly visible ripe fruit, designed for isometric farm depth sorting.
3. `moonmelon.png` (or another unique icon filename): standalone fruit icon for Market and harvest pickup.
4. Source working file(s), palette notes, licence/provenance record, and a preview at actual in-game display scale.

The current SVG crop study is a design reference, not a substitute for these three runtime images. Before packing, inspect several neighboring crop assets and match their canvas, pivot/ground contact, baked-soil treatment, and alpha convention. Do not guess dimensions from the concept sheet.

## Zombie: Glowcap

The existing special-zombie path is preferable to inventing a new actor renderer. The runtime contract is `SpecialZombieManifest` in `src/assets.ts`:

- `name`, `neck`, and `parts[]` define a rig delta over an existing skeleton (or a complete actor only when genuinely required).
- Each part names a source file and supplies the existing `ZombieModelPart` placement fields.
- Optional `flipbook.idle[]`, `flipbook.attack[]`, and `fps` provide texture-swap animation.
- `specialZombieFiles()` collects all part and flipbook filenames for loading.

**Art deliverables**
1. A friendly, readable mushroom-cap silhouette that remains identifiable at the farm's actual small display scale.
2. Separate body/head/cap/face/arm parts only where the existing rig needs them; do not flatten a rigged character into one static image.
3. A four-pose idle flipbook study: neutral, gentle bob up, peak bob, settle.
4. A short attack flipbook study: anticipation, cap glow, spore puff, recovery.
5. A rig placement sheet with pivot/neck reference marks.

The exact source directory, base actor, offsets, scale, tint rules, filenames, and fps must be chosen by comparing existing special-zombie manifests and source art. Never invent offsets from the concept SVG. If a single complete sprite is chosen instead, document why it cannot use the ordinary skeleton.

## Mutation: Mooncap Crown

Mutation rules are in `src/zombie/mutations.ts`; runtime attachment data is in `public/assets/zombie/mutations.json` and `MutationPart` in `src/assets.ts`.

Before implementation, maintainer approval is required. If approved:
- Append a stable key to the end of the catalog; never insert, reorder, or reuse an existing mutation key/bit.
- Choose a slot only after checking the visible part and existing slot conventions. The current concept is a head-slot crown, but the catalog's one-mutation-per-slot rule means it conflicts with other head mutations.
- Add a matching key in `mutations.json` with a valid existing atlas frame or a correctly prepared loose file, plus `group`, `headRel`, offsets, anchors, z-order, and `replaces` only if the part truly replaces a base silhouette.
- Add crop acquisition in `src/zombie/cropMutations.ts` only if the approved acquisition design requires it.
- Add tests for append-only bit stability, slot legality, render attachment, combine inheritance, and old-save compatibility.

No stat bonus or penalty is proposed here. It requires balance review; do not infer one from the art.

## Bosses and decorations

Bosses use the raid-enemy rig pipeline, not the ordinary zombie skeleton. Read `tools/prep_enemies.py`, `EnemyModel`, and the existing raid enemy manifests before producing parts. Each boss needs original part art, a rig, attack telegraphs, attack timing, and combat tests. The current boss sheet is silhouette/telegraph direction only.

The first decoration set should remain cosmetic. Inspect the existing placeable/object catalog and placement footprint conventions before adding any runtime decoration. No passive bonuses, storage, collision, or economy effects are implied by the current concepts.

## Atlas/cache correctness gate

Zombie atlas frame tables and PNGs are a matched pair. Every packer must call `stamp_frames` from `tools/atlas_stamp.py` after saving the PNG. The stamp binds the frame JSON to the sheet digest; `src/atlasVersion.test.ts` checks the committed pair. Never hand-edit frame rectangles or the digest.

## Review checklist before any runtime asset PR

- [ ] Maintainer has approved the specific vertical-slice scope.
- [ ] All new source art has recorded provenance/licence.
- [ ] Crop images match existing canvas, alpha, pivot, and baked-soil conventions.
- [ ] Crop icon is a separate standalone harvest/Market image.
- [ ] Glowcap uses the existing special-zombie rig/flipbook contract where practical.
- [ ] Mooncap Crown uses a legal slot and an append-only mutation key.
- [ ] Atlas and frame table are regenerated together and stamped.
- [ ] No generated pack output is committed in place of its source/prep script.
- [ ] Client catalog, Almanac, Market, Local Farm acquisition, and tests agree.
- [ ] Existing saves load unchanged.
- [ ] `npm test && npm run build` pass.
- [ ] iPhone Safari Home Screen install, portrait/landscape, touch controls, and offline Local Farm are tested.

## Current implementation boundary

The concept SVGs under `docs/concepts/` are original design references only. Nothing in this document is wired into the game. This boundary stays in place until the project maintainer approves the gameplay/content approach and the production assets are prepared through the existing pipeline.
