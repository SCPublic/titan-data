# Engine-Kill app overrides

JSON files in this folder customize how titans work in the Engine Kill app. The app fetches them from the titan-data repo and uses them instead of hardcoded values when available.

| File | Purpose |
|------|---------|
| `chassis-overrides.json` | Plasma reactor max, void shields max, void shield saves per chassis (template id). Used when BattleScribe XML doesn't provide these. |
| `damage-tracks.json` | Per-chassis damage: armor, hit table, critical effects for head/body/legs. Used for the command terminal / damage track UI. |
| `weapon-metadata.json` | Per-weapon UI metadata keyed by `"name\|mountType"` (lowercase): `repairRoll`, `disabledRollLines`. Used for the "Weapon Disabled" overlay. |

All keys use the app's template/chassis ids (e.g. `warhound`, `reaver`, `warlord`, `warmaster`). The app falls back to bundled data if a file is missing or the fetch fails.
