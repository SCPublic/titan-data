# Engine-Kill app overrides

JSON files in this folder customize how titans work in the Engine Kill app. The app fetches them from the titan-data repo and uses them instead of hardcoded values when available.

## Generated app-ready payload (templates.json)

The **Engine Kill app** can load a single file instead of merging XML and overrides at runtime:

- **File:** `engine-kill/generated/templates.json` (see [generated/README.md](generated/README.md) for the payload shape).
- **How it’s produced:** A generator script in the engine-kill repo runs the same merge logic (BattleScribe XML + these override JSONs) and writes `generated/templates.json`. Run it whenever XML or override files change.
- **From engine-kill repo:**  
  `TITAN_DATA_OUTPUT=/path/to/titan-data/engine-kill/generated/templates.json npm run generate-templates`  
  Or, with titan-data as a sibling:  
  `TITAN_DATA_OUTPUT="../titan-data/engine-kill/generated/templates.json" npm run generate-templates`
- **From this repo (titan-data):**  
  From `engine-kill/scripts/`: `./run-generator.sh` (requires engine-kill cloned as a sibling of titan-data).

After the first run, you can maintain `generated/templates.json` by hand if you prefer; the generator is for one-time migration or periodic sync.

| File | Purpose |
|------|---------|
| `chassis-overrides.json` | Plasma reactor max, void shields max, void shield saves per chassis (template id). Optional `specialRules` (array of strings) for chassis that don't have rules in BattleScribe (e.g. Warhound Squadron). |
| `banner-overrides.json` | Per-banner overrides for Knight/Stalker banners. Keys = banner template id using the same pattern as titans: `bs:${selectionEntryId}` (BattleScribe entry id). See [Banner overrides schema](#banner-overrides-schema) below. |
| `critical-effects.json` | **Single source for default critical effects** for head, body, and legs (all titans). Head: MIU Feedback → Moderati Wounded → Princeps Wounded; body: Reactor Leak / VSG Burnout; legs: Stabilisers / Locomotors / Immobilised. Omit `criticalEffects` in damage-tracks to use these defaults. Override only for special cases (e.g. Warmaster head has 4 crit tracks; first two share the same effect as level 1). |
| `damage-tracks.json` | Per-chassis damage: armor, **armor rolls** (Direct/Devastating/Critical roll ranges per location). Omit `criticalEffects` to use defaults from `critical-effects.json`. Use `armorRolls: { "direct": "11-13", "devastating": "14-15", "critical": "16+" }` per location (legacy `hitTable` still supported). When adding new chassis you can omit `critical` and assume it is one step above devastating (e.g. devastating `"15-16"` → critical `"17+"`). |
| `weapon-metadata.json` | Per-weapon UI metadata keyed by `"name\|mountType"` (lowercase): `repairRoll`, `disabledRollLines`. Used for the "Weapon Disabled" overlay. |

All keys use the app's template/chassis ids (e.g. `warhound`, `reaver`, `warlord`, `warmaster`). The app falls back to bundled data if a file is missing or the fetch fails.

### Banner overrides schema

`banner-overrides.json` holds only data **not** present in BattleScribe (Engine stats, Ion Shields, rules, and weapons come from the .gst). One entry per banner, keyed by the banner template id in the same way as titans: **`bs:${selectionEntryId}`** (the BattleScribe `selectionEntry` id from the .gst). Optional `banner-aliases.json` can map `bs:entryId` to a different id if needed.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `structurePointsMax` | number | Yes | Total structure points for the banner. |
| `minKnights` | number | No | Minimum knights in the banner. |
| `maxKnights` | number | No | Maximum knights in the banner. |
| `bannerBasePoints` | number | No | Base points cost for the banner. |
| `bannerPointsPerKnight` | number | No | Points per knight. |

A banner that exists in the .gst but has no entry here will cause the app to surface a warning (no silent fallback).
