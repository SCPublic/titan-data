# Engine Kill — `templates.json`

The [Engine Kill](https://github.com/SCPublic/engine-kill) app loads **one** file from this repo at runtime:

**`templates.json`** (repository root)

It must include `titans`, `banners`, and may include `maniples`, `legions`, `upgrades`, `princepsTraits`, `warnings` (arrays). Shape matches engine-kill types; see engine-kill [`docs/DATA_PATTERNS.md`](https://github.com/SCPublic/engine-kill/blob/main/docs/DATA_PATTERNS.md) and [`src/models/UnitTemplate.ts`](https://github.com/SCPublic/engine-kill/blob/main/src/models/UnitTemplate.ts). For agents: [`docs/AGENT_DATA_CONTEXT.md`](https://github.com/SCPublic/engine-kill/blob/main/docs/AGENT_DATA_CONTEXT.md).

**Editing:** Change game data by editing `templates.json` directly and committing. No generator or override JSON is maintained in this repo for that workflow.

**Changelog (high level):** 2026-03-29 — All `banners[]` entries now use real **`bs:`** weapon rows in `availableWeapons` (from `Adeptus Titanicus 2018.gst` **Weapon** profiles); the synthetic **`placeholder`** weapon id is no longer used on banners. **Acastus:** `acastus-knight-banner` lists **Asterius** mortar / conversion / volkite plus **Porphyrion** arm options only (no hull autocannon/lascannon); **`acastus-knight-porphyrion-banner`** carries **Porphyrion** hull + arm weapons; **`acastus-knight-asterius-banner`** remains the dedicated Asterius banner build. **BattleScribe:** `Acastus Knight Porphyrion` in `Adeptus Titanicus 2018.gst` had **Arm Weapon** choices as two separate required links (Twin Magna *and* Ironstorm); it is now an **Arm Weapon** group with **min 1 / max 1** so you pick one arm weapon, matching the rules (fixes invalid roster / blocked adds in BattleScribe).

**Questoris Knight Banner:** `availableWeapons` matches **Questoris Knight Lord Scion** in the `.gst` — four arm options (Avenger, Thermal, Rapid-Fire Battlecannon, Questoris Melee Weapon) plus **Stormspear Rocket Pod** as **`mountType` `carapace`**. Meltagun is omitted (catalogue **Upgrades** / special attack, not a weapon profile). Spurious entries copied from other kits (e.g. Styrix/Magaera arms, roster models) were removed.

**Questoris Styrix / Magaera banners:** `questoris-knight-styrix-banner` — **Volkite Chieorovile**, **Hekaton Siege Claw** (per **Lord Styrix** arms in `.gst`). `questoris-knight-magaera-banner` — **Lightning Cannon**, **Hekaton Siege Claw** (per **Lord Magaera**). **`hasCarapaceWeapon`:** `false`. Graviton gun / Phased Plasma-Fusil are catalogue upgrades (special attacks), not listed as `availableWeapons`.

**Knight banners with fixed arms (e.g. Cerastus Atrapos):** In `banners[]`, supply the two arm weapons in `availableWeapons` (BattleScribe `bs:` ids), set `fixedBannerArmWeaponIds` to `[firstArm, secondArm]` for every knight, and set `minKnights` / `maxKnights` / `bannerBasePoints` / `bannerPointsPerKnight` from the catalogue. Same pattern for any chassis with no arm options. See engine-kill [`docs/DATA_PATTERNS.md` §4](https://github.com/SCPublic/engine-kill/blob/main/docs/DATA_PATTERNS.md).

**URL (GitHub raw):**  
`https://raw.githubusercontent.com/SCPublic/titan-data/master/templates.json`
