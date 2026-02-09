# Engine-Kill app overrides

JSON files in this folder customize how titans work in the Engine Kill app. The app fetches them from the titan-data repo and uses them instead of hardcoded values when available.

| File | Purpose |
|------|---------|
| `chassis-overrides.json` | Plasma reactor max, void shields max, void shield saves per chassis (template id). Used when BattleScribe XML doesn't provide these. |
| `critical-effects.json` | **Single source for default critical effects** for head, body, and legs (all titans). Head: MIU Feedback → Moderati Wounded → Princeps Wounded; body: Reactor Leak / VSG Burnout; legs: Stabilisers / Locomotors / Immobilised. Omit `criticalEffects` in damage-tracks to use these defaults. Override only for special cases (e.g. Warmaster head has 4 crit tracks; first two share the same effect as level 1). |
| `damage-tracks.json` | Per-chassis damage: armor, **armor rolls** (Direct/Devastating/Critical roll ranges per location). Omit `criticalEffects` to use defaults from `critical-effects.json`. Use `armorRolls: { "direct": "11-13", "devastating": "14-15", "critical": "16+" }` per location (legacy `hitTable` still supported). When adding new chassis you can omit `critical` and assume it is one step above devastating (e.g. devastating `"15-16"` → critical `"17+"`). |
| `weapon-metadata.json` | Per-weapon UI metadata keyed by `"name\|mountType"` (lowercase): `repairRoll`, `disabledRollLines`. Used for the "Weapon Disabled" overlay. |

All keys use the app's template/chassis ids (e.g. `warhound`, `reaver`, `warlord`, `warmaster`). The app falls back to bundled data if a file is missing or the fetch fails.
