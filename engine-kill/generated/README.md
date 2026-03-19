# Generated templates payload

This directory holds the **app-ready** JSON consumed by the Engine Kill app. The app fetches a single file (e.g. `templates.json`) and uses it as the only source for all template data—no override loading or runtime merge.

## Payload shape

The generated file must conform to the following contract. Types are defined in the engine-kill repo; this spec is the contract for the generator and the app.

### Root: `TemplatesPayload`

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `titans` | `UnitTemplate[]` | yes | Titan chassis (Reaver, Warlord, etc.). Same shape as engine-kill `UnitTemplate` with `unitType: 'titan'`. |
| `banners` | `UnitTemplate[]` | yes | Knight banners (Questoris, Cerastus, etc.). Same shape with `unitType: 'banner'` and banner-only fields (`minKnights`, `maxKnights`, `bannerBasePoints`, `bannerPointsPerKnight`, etc.). |
| `maniples` | `ManipleTemplate[]` | no | Maniple definitions (from Battlegroup.cat). |
| `legions` | `LegionTemplate[]` | no | Legion definitions (Legio Mortis, etc.). |
| `upgrades` | `UpgradeTemplate[]` | no | Titan/banner upgrades. |
| `princepsTraits` | `PrincepsTraitTemplate[]` | no | Princeps trait options. |
| `warnings` | `string[]` | no | Optional generator warnings (e.g. missing data). App may surface these. |

### UnitTemplate (titans and banners)

Defined in engine-kill `src/models/UnitTemplate.ts`. Summary:

- **Common:** `id`, `name`, `unitType` (`'titan'` \| `'banner'`), `defaultStats` (void shields, heat, plasma reactor, damage head/body/legs with `max` and `armorRolls`, `hasCarapaceWeapon`, `stats`), `availableWeapons` (array of `WeaponTemplate`).
- **Titan-only (optional):** `scale`, `scaleName`, `basePoints`, `specialRules`, `defaultLeftWeaponId`, `defaultRightWeaponId`.
- **Banner-only (optional):** `minKnights`, `maxKnights`, `bannerBasePoints`, `bannerPointsPerKnight`; `defaultStats` may include `structurePointsMax`, `ionShieldSaves`.

### WeaponTemplate

Part of `UnitTemplate.availableWeapons`. Fields: `id`, `name`, `points`, `shortRange`, `longRange`, `accuracyShort`, `accuracyLong`, `dice`, `strength`, `traits`, `specialRules`, `mountType` (`'arm'` \| `'carapace'`), optional `repairRoll`, `disabledRollLines`, `legioKeys`.

### Other template types

- **ManipleTemplate**, **LegionTemplate**, **UpgradeTemplate**, **PrincepsTraitTemplate**: See engine-kill `src/models/` and adapter result types in `battlescribeAdapter.ts` (`BattleScribeManiplesLoadResult`, etc.). The generator must output the same shapes the app currently gets from the BattleScribe loader.

## ID scheme

- **Template id:** Stable human-readable slugs (e.g. `reaver`, `warlord`, `questoris`). Not raw BattleScribe IDs like `bs:dfeb-83af-7b26-622a` in the payload (the app may still map legacy BS ids to these via constants).
- **Weapon id:** Human-readable slug derived from weapon name (e.g. `laser-blaster`, `volcano-cannon`).

## How this file is produced

A **generator script** (one-time migration) runs in titan-data: it reads the `.gst` / `.cat` XML and the `engine-kill/*.json` override files, runs the same merge logic engine-kill uses today (or calls engine-kill’s adapter), and writes `templates.json` here. After the initial run, titan-data can maintain `templates.json` directly; re-run the generator whenever XML or overrides change.

See `engine-kill/README.md` in this repo for how to run the generator and how the app consumes this file.
