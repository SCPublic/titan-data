# Engine Kill — `templates.json`

The [Engine Kill](https://github.com/SCPublic/engine-kill) app loads **one** file from this repo at runtime:

**`templates.json`** (repository root)

It must include `titans`, `banners`, and may include `maniples`, `legions`, `upgrades`, `princepsTraits`, `warnings` (arrays). Shape matches engine-kill types (`UnitTemplate`, etc.); see engine-kill [`docs/TITAN_DATA_GENERATED_SHAPE.md`](https://github.com/SCPublic/engine-kill/blob/main/docs/TITAN_DATA_GENERATED_SHAPE.md) if present, or `src/models/UnitTemplate.ts`.

**Editing:** Change game data by editing `templates.json` directly and committing. No generator or override JSON is maintained in this repo for that workflow.

**URL (GitHub raw):**  
`https://raw.githubusercontent.com/SCPublic/titan-data/master/templates.json`
