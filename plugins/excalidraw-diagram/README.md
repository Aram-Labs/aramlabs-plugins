# excalidraw-diagram

Generate Excalidraw diagrams — architecture, flows, concepts — that make a visual argument. Not just boxes-and-arrows: every shape mirrors the concept it represents, with real evidence (code snippets, event names, JSON payloads) embedded. Built around a render → inspect → fix validation loop so diagrams actually land.

Invoked via `/diagram` or natural language ("Create an Excalidraw diagram showing…"). Outputs a `.excalidraw` JSON file plus a rendered PNG.

## Install

```bash
/plugin marketplace add Aram-Labs/aramlabs-plugins   # skip if already added
/plugin install excalidraw-diagram
```

## One-time dependency setup

The render-validate loop uses Python + Playwright (headless Chromium). After installing the plugin, run once:

```bash
cd ~/.claude/plugins/aramlabs-plugins/plugins/excalidraw-diagram/skills/excalidraw-diagram/references
uv sync
uv run playwright install chromium
```

Requires [`uv`](https://github.com/astral-sh/uv) (`brew install uv` if missing).

## Usage

```
/diagram Create a diagram showing how our ingestion pipeline routes events from Segment through Kafka to Snowflake
```

Or conversationally:

> "Diagram the auth flow — browser → Supabase → Next.js middleware → Postgres."

The skill will produce both a `.excalidraw` JSON (open/edit at [excalidraw.com](https://excalidraw.com)) and a rendered PNG next to it.

## Credit

Based on [`coleam00/excalidraw-diagram-skill`](https://github.com/coleam00/excalidraw-diagram-skill). Aram Labs customizations: none yet — color palette and element templates are currently upstream defaults. Future tweaks (brand palette, standard element templates) will land in `skills/excalidraw-diagram/references/`.

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| `uv: command not found` | `brew install uv` |
| `playwright install` fails | Re-run `uv run playwright install chromium`; on Apple Silicon ensure Rosetta isn't required |
| PNG renders empty / white | Delete the `.excalidraw` and re-run — malformed JSON is the usual cause. The skill will re-validate |
| Skill not found after install | Restart Claude Code; verify `~/.claude/plugins/.../excalidraw-diagram/` exists |
