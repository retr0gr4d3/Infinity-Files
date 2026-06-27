# Infinity-Files

Community content for **Beyond** (the Infinity client). Two kinds of content live here:

- **Scripts** — quest chains for the Quest Runner / Automation.
- **Autoskills** — saved skillset rotations for the Autoskills engine.

Beyond pulls this repo from inside the client: open the **Library** window and click
**Update Library**. The contents are downloaded into
`<game dir>/UserData/Beyond/Infinity-Files/` and listed so each script or autoskill can be
loaded individually. Loading a script merges it into your `chains.json`; loading an autoskill
imports it into your saved skillsets — both exactly as if you'd imported them by hand.

---

## Scripts (`Scripts/*.json`)

**One file per quest chain.** The file name (without `.json`) is just for the Library list; the
chain name comes from the top-level key. To add a new chain, drop a new `<Name>.json` in `Scripts/`.

```json
{
  "Lair": [
    { "qid": 19, "area": "lair", "frame": "Enter", "pad": "Spawn", "items": 1 },
    { "qid": 20, "area": "lair", "frame": "r2",    "pad": "Spawn", "items": 1 }
  ]
}
```

Per entry:

| field   | type   | meaning                                                                 |
|---------|--------|------------------------------------------------------------------------|
| `qid`   | int    | quest id to accept / hunt / turn in (required, > 0)                     |
| `area`  | string | area to `tfer` into; `""` = stay in current area                       |
| `frame` | string | cell to move to; `""` = stay in current cell                           |
| `pad`   | string | spawn pad in the target cell (default `"Spawn"`)                        |
| `items` | int    | how many times to loop this quest (default `1`)                        |

A file may contain more than one chain (multiple top-level keys); loading it imports all of them.
Any extra keys (e.g. `_note`) are ignored, so you can annotate entries inline.

## Autoskills (`Autoskills/autoskills.txt`)

**One file, many entries — one rotation per line**, in the Autoskills import/export format:

```
Name|Combo|Delays|Waits|Frees
Generic|1,2,3,4,5|1000,1000,1000,1000,1000|False,False,False,False,False|False,False,False,False,False
```

| field    | meaning                                                                       |
|----------|-------------------------------------------------------------------------------|
| `Name`   | display / class name (first field; shown in the Library list)                 |
| `Combo`  | skill slots to cycle, e.g. `1,2,3,4,5`                                         |
| `Delays` | per-skill delay in ms (5 values)                                              |
| `Waits`  | per-skill "wait for the skill to land before continuing" flags (5 booleans)  |
| `Frees`  | per-skill "free cast / ignore global cooldown" flags (5 booleans)            |

Blank lines and lines starting with `#` or `//` are ignored, so the file can carry a header and
comments. Add a new rotation by appending another line.
