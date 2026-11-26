# Expanded Edition — content expansion plan

**Status: proposal, not shipped gameplay.** The first implementation milestone is the separate modern HUD theme in `src/ui/expandedEdition.css`. This document records the proposed content direction before any new gameplay, save, economy, or combat rules are introduced.

The target is a modern-HD presentation that still reads as a playful, colourful zombie-farming game. The existing farm, roster, crop, mutation, quest, raid, and save systems remain the foundation.

## Product principles

1. **Keep the farm loop first.** Plant, wait, harvest, collect, decorate, and grow the roster should remain the centre of play.
2. **Readable at phone size.** New art must remain legible on a narrow iPhone screen; critical actions need comfortable touch targets.
3. **Additive progression.** New content should extend existing unlocks and collection goals rather than invalidate existing farms or make existing zombies obsolete.
4. **Local-first compatibility.** A player must be able to enjoy new content in Local Farm without requiring an account or a server.
5. **Provenance-safe art.** New Expanded Edition characters, crops, decorations, icons, and audio must be original commissioned/generated assets or assets with a documented compatible licence. Do not extract or redistribute commercial-game assets.
6. **No save breakage.** Any persistent content requires a versioned migration and tests before it can ship.

## Proposed original content

### New zombies

| Concept | Visual identity | Gameplay role |
|---|---|---|
| Mossback | A squat garden guardian with mossy shoulders and tiny mushrooms | Durable defender; slow grow |
| Glowcap | A small night-glowing mushroom zombie | Support / team utility |
| Compost Golem | A chunky soil-and-root construct | Heavy front-line bruiser |
| Lantern Wisp | A floating will-o'-the-wisp with leaf-like wisps | Fast, fragile specialist |

Names and roles are working concepts. Final stats, rarity, cost, grow time, unlock level, and combination rules need balance review and tests before implementation.

### New crops

| Crop | Visual stages | Intended hook |
|---|---|---|
| Moonmelon | Seedling → curling vine → luminous melon | A new high-level growable crop |
| Glowcap Cluster | Tiny caps → clustered glowing mushrooms | A support-themed zombie affinity |
| Rootglass | Crystal-like roots → faceted harvest | A rare cosmetic/economy harvest |
| Graveberry | Dark berry bush → bright spectral fruit | A mutation-adjacent late-game crop |

Each crop needs a complete authored data path: market definition, growth/harvest behavior, crop art at each required stage, unlock and price rules, and tests. Mutation-bearing crops additionally need a documented mutation slot and stat trade-off.

### Mutations

The first mutation proposals should be visually distinct, occupy existing legal slots, and have a meaningful trade-off rather than only power creep:

- **Mooncap Crown** — head slot; modest Constitution bonus with a small Dexterity penalty.
- **Bramble Guard** — arm slot; defensive identity with reduced Dexterity.
- **Rootweave Mantle** — body slot; Constitution-focused, modest Strength trade-off.
- **Wisp Eyes** — hair/eye slot; Dexterity-focused, lower Constitution.

Mutation IDs are persisted bit positions. The catalog is append-only: never reorder or reuse an existing bit. A new mutation is not complete until it has compatible art, crop acquisition (if applicable), roster/almanac display, combine behavior, and tests. Do not append placeholder mutations that have no working visual or acquisition route.

### Boss encounters

- **The Rotwood Keeper** — a towering orchard guardian with root-and-vine attacks; introduces telegraphed area control.
- **The Lantern Queen** — a floating nocturnal boss with summoned wisps; tests target priority and team composition.
- **The Compost Colossus** — a slow armoured boss with a heavy slam; tests durable teams and attack timing.

These are original encounter concepts, not yet RaidCatalog entries. Each boss needs original enemy art and a rig, authored attacks/telegraphs, a difficulty curve, rewards, accessibility/readability review, and deterministic combat tests. Do not reuse existing raid rewards or tune boss HP/damage by intuition alone.

### Farm decorations

- Moonlit greenhouse (large functional-looking visual centrepiece; decorative until a real system is specified).
- Mushroom lanterns (small footprint, warm/cool glow variants).
- Root arch and mossy stone path pieces (farm layout tools).
- Tiny animated pond spirits (ambient decoration, no gameplay effect).

Decorations should be cosmetic unless their gameplay effect, placement footprint, price, storage behavior, and server authority are explicitly designed.



### Original visual concept assets (pre-production)

### iPhone release validation
- `docs/IOS_INSTALL_AND_QA.md` records the Safari Home Screen installation flow, device acceptance matrix, and release gates. It is a test plan only; no physical iPhone test is claimed.

### Production implementation spec
- `docs/PRODUCTION_ART_SPEC.md` maps the Moonmelon, Glowcap, Mooncap Crown, boss, and decoration concepts to the repository's existing runtime contracts, asset paths, atlas stamping, provenance rules, and review checklist.
- `docs/concepts/glowcap-animation.svg` is a pose-blocking storyboard for an idle loop and spore-burst attack; it is not a runtime flipbook or production sprite sheet.


- `docs/concepts/moonmelon.svg` — original three-stage crop study (sprout, vine, ripe fruit).
- `docs/concepts/moonmelon-production-sheet.svg` — separate stage-1, stage-2, and standalone harvest-icon art direction for the crop vertical slice; vector study only.
- `docs/concepts/glowcap.svg` — original friendly support-zombie character study.
- `docs/concepts/mooncap-crown.svg` — original head-slot mutation study, showing the intended replaceable crown silhouette.
- `docs/concepts/moonlit-decorations.svg` — original greenhouse, mushroom lantern, root arch, and pond-spirit studies; cosmetic concepts only.
- `docs/concepts/boss-roster.svg` — original Rotwood Keeper, Lantern Queen, and Compost Colossus silhouette/telegraph studies; no combat data or rewards.
- `docs/BOSS_PRODUCTION_BRIEF.md` maps the three boss concepts onto the existing raid-enemy rig/prep pipeline; it is a pre-production brief only.
- These are design references only, not production sprites or wired game content. They have no gameplay, save, economy, or server effects. Production art must be adapted to the repository's sprite pipeline and reviewed before integration.

## Delivery sequence

### Milestone 1 — modern presentation (in progress)
- Separate HUD theme layer.
- Brand the installable app shell as “Zombie Farm 2: Expanded Edition” in `index.html` and `public/manifest.webmanifest`; the existing PWA registration, standalone display mode, safe-area viewport, and local-first save behavior remain in place.
- Add an original SVG Expanded Edition icon for browsers and supporting PWA clients (`public/icons/expanded-edition.svg`), update the app theme color, and label the launch screen. The iOS-specific `apple-touch-icon.png` remains the repository's existing image until a properly sized original PNG can be produced.
- Keep existing Pixi rendering and controls intact.
- Validate narrow portrait, landscape, desktop, focus states, and reduced-motion behavior.
- Run `npm test` and `npm run build`.

### Milestone 2 — art and content pipeline
- Create original concept sheets and production-ready sprite assets.
- Document provenance and licences for every new asset.
- Add a reproducible asset-preparation path; do not commit generated output when the repository expects a prep script instead.

### Milestone 3 — one vertical slice
Build one complete small content path before adding a large catalog:
1. One new crop with seedling/growing/harvest art.
2. One zombie obtainable from that crop.
3. One mutation with a legal slot, balanced stats, and visible attachment art.
4. Almanac/Market/quest integration and tests.
5. Verify old saves load unchanged.

### Milestone 4 — boss and decoration expansion
- Add one original boss end-to-end, including its rig, attacks, rewards, and tests.
- Add a small first decoration set.
- Expand only after the vertical slice passes on Local Farm and the maintainer has reviewed the design.

## Engineering gates

Before any gameplay content is merged:

- Follow `CONTRIBUTING.md`: the repository asks for maintainer agreement before non-trivial gameplay changes. The repository's Issues feature is disabled, so obtain agreement through the project's accepted contribution channel before implementing gameplay rules.
- Keep server catalogs and client catalogs in sync; prefer deriving server catalogs from the source asset when the project already does so.
- Add tests for crop unlocks, costs, mutation bit stability, slot legality, combine outcomes, and combat where applicable.
- Update `README.md` and `docs/FEATURES.md` in the same change when behavior ships.
- Run the client checks (`npm test && npm run build`) and relevant server checks if server code or catalogs change.
- Test the installed PWA on iPhone Safari, including Home Screen launch, safe areas, portrait/landscape, touch interactions, and offline Local Farm.

## Explicit non-goals

- Replacing the existing game with a separate unrelated prototype.
- Rewriting the isometric renderer or save format for cosmetic changes.
- Claiming this fan project is official or affiliated with the original publisher.
- Shipping extracted original-game art/audio or assets of unclear provenance.
