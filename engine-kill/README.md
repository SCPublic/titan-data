# Engine-Kill app data

## Canonical artifact: templates.json

The **Engine Kill app** loads a single file from titan-data at runtime: **`engine-kill/generated/templates.json`**. That file is the **canonical source** for all template data (titans, banners, maniples, legions, upgrades, princeps traits). The app does **not** fetch any other engine-kill JSON (no override files at runtime).

- **File:** [engine-kill/generated/templates.json](generated/templates.json) (see [generated/README.md](generated/README.md) for the payload shape).
- **Updates:** Edit `generated/templates.json` directly when game data changes, or use a titan-data–owned build that produces it. The engine-kill repo may still provide a generator script for one-time or transitional use; the long-term goal is for titan-data to own this file. See engine-kill [docs/REFACTOR_PROGRESS.md](https://github.com/SCPublic/engine-kill/blob/main/docs/REFACTOR_PROGRESS.md) for the refactor (single JSON, no overrides).

### Legacy override files (used only by generator during transition)

The following files are **not** fetched by the app at runtime. They are used only when running the generator (e.g. from engine-kill) to produce `templates.json`. They will be removed or archived once the refactor is complete and titan-data owns `templates.json` as the only artifact.

| File | Purpose |
|------|---------|
| `chassis-overrides.json` | Plasma reactor max, void shields max, void shield saves per chassis (template id). Optional `specialRules` (array of strings) for chassis that don't have rules in BattleScribe (e.g. Warhound Squadron). |
| `banner-overrides.json` | Per-banner overrides for Knight/Stalker banners. Keys = banner template id using the same pattern as titans: `bs:${selectionEntryId}` (BattleScribe entry id). See [Banner overrides schema](#banner-overrides-schema) below. |
| `critical-effects.json` | **Single source for default critical effects** for head, body, and legs (all titans). Head: MIU Feedback → Moderati Wounded → Princeps Wounded; body: Reactor Leak / VSG Burnout; legs: Stabilisers / Locomotors / Immobilised. Omit `criticalEffects` in damage-tracks to use these defaults. Override only for special cases (e.g. Warmaster head has 4 crit tracks; first two share the same effect as level 1). |
| `damage-tracks.json` | Per-chassis damage: armor, **armor rolls** (Direct/Devastating/Critical roll ranges per location). Omit `criticalEffects` to use defaults from `critical-effects.json`. Use `armorRolls: { "direct": "11-13", "devastating": "14-15", "critical": "16+" }` per location (legacy `hitTable` still supported). When adding new chassis you can omit `critical` and assume it is one step above devastating (e.g. devastating `"15-16"` → critical `"17+"`). |
| `weapon-metadata.json` | Per-weapon UI metadata keyed by `"name\|mountType"` (lowercase): `repairRoll`, `disabledRollLines`. Used for the "Weapon Disabled" overlay. |

All keys use the app's template/chassis ids (e.g. `warhound`, `reaver`, `warlord`, `warmaster`).

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
