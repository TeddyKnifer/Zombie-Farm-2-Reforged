# Expanded Edition — boss production pipeline brief

**Status: pre-production technical brief only.** No boss parts, combat rules, rewards, raid catalog entries, or runtime files are added by this document.

## Existing enemy pipeline

The repository already has a rigged raid-enemy path. Use it rather than creating a parallel boss renderer.

- Source rig and art preparation: `tools/prep_enemies.py`.
- Runtime model contract: `EnemyModel` and `EnemyPart` in `src/assets.ts`.
- Runtime model data: `public/assets/raids/enemies/models.json`.
- Runtime enemy scene uses the model's part strips and procedural animation.
- The current prep tool reads recovered stage sheets and their rigs from the sibling `ZF2R_extracted/` working tree. New original art must be supplied through an explicitly documented original-art input path; do not place it in or imply it came from the extracted commercial assets.
- Existing enemy models demonstrate the coordinate convention: `rx/ry/rw/rh` slice a packed part strip; `px/py` place the part; `ax/ay` are anchors; `z` controls order; `rot` is display rotation in radians; `group` selects procedural motion; `back` selects behind-body drawing.

## Boss art deliverables

Every boss needs a consistent part kit and a preview at actual raid-scene size. Minimum suggested part groups are body, head, back/front arms, and legs; add wings, roots, lanterns, or armour only when the silhouette requires them.

### Rotwood Keeper
- Silhouette: broad trunk torso, branch crown, root-like legs, long vine arms.
- Parts: trunk/body, face/head, branch crown, rear/front root arms, rear/front root feet.
- Animation read: slow body sway; root legs should remain grounded; vine arms must not obscure the face.
- Attack telegraph: marked ground lanes and a brief root-lift pose before a root slam.

### Lantern Queen
- Silhouette: narrow floating torso, crown/lantern halo, long trailing wisps.
- Parts: torso, face/head, crown, paired wisps/tendrils, optional floating lantern nodes.
- Animation read: hover loop with low-amplitude vertical motion; wisps move independently only if the rig supports a distinct group.
- Attack telegraph: summon circles appear before the wisp projectiles/actors enter. Do not imply damage or target selection until combat rules are approved.

### Compost Colossus
- Silhouette: wide low centre of mass, heavy shoulders, compost armour plates.
- Parts: torso, head, shoulder/arm plates, rear/front arms, legs, visible compost/moss accents.
- Animation read: slow weight shift and clearly planted feet.
- Attack telegraph: widening ground ring before a heavy slam.

## Runtime and gameplay gates

The concept names and attack ideas are not a balance or encounter specification. Before runtime work:
1. Obtain maintainer agreement through the repository's accepted contribution channel. The Issues feature was unavailable during the current check; do not treat silence as approval.
2. Confirm which existing raid stage/encounter slot is intended and whether this is a Local Farm-only preview or a full raid addition.
3. Define authoritative health, damage, attack cadence, hit windows, target rules, drops, unlocks, and replay rules using recovered project conventions.
4. Author original part art and record its provenance/licence.
5. Extend `tools/prep_enemies.py` only through a reproducible original-art input path; preserve the existing recovered-art input behavior.
6. Generate packed parts and model data together. Do not hand-edit generated rectangles or offsets.
7. Add tests for model references, bounds, draw order, and attack behavior; update README Current Gaps and `docs/FEATURES.md` in the same behavior-changing change.
8. Run client tests/build and relevant server tests if the authoritative encounter or reward path changes.
9. Verify actual raid rendering and touch behavior on iPhone Safari.

## Scope recommendation

Start with **one** boss only after the crop/zombie/mutation vertical slice is accepted and validated. Rotwood Keeper is the clearest first rig because its trunk, head, arms, and feet map naturally to the existing `EnemyModel` groups. Treat this as an art-pipeline recommendation, not approval to change encounter data.
