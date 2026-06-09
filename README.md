# ShipToday Marketplace for Claude Code

The official [Claude Code](https://code.claude.com) plugin marketplace from
**ShipToday**.

## Available plugins

| Plugin | Description |
| ------ | ----------- |
| [`forge-shiptoday`](plugins/forge-shiptoday) | Free, AI-powered product development lifecycle automation. Just describe what you want to build — Forge routes your feature requests, bug reports, and architecture explorations through structured PDLC workflows. No trigger words needed. |

## Installation

Add this marketplace to Claude Code, then install a plugin from it:

```
/plugin marketplace add ShipToday/forge-plugin-claude-marketplace
/plugin install forge-shiptoday@shiptoday
```

The first command registers the marketplace (referenced by its name,
`shiptoday`). The second installs the Forge plugin from it. Browse everything
available with `/plugin`.

To update later:

```
/plugin marketplace update shiptoday
```

## Repository structure

```
forge-plugin-claude-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace manifest (lists the plugins)
├── plugins/
│   └── forge-shiptoday/          # The Forge plugin
│       ├── .claude-plugin/
│       │   └── plugin.json       # Plugin manifest
│       ├── .mcp.json             # Forge MCP server definition
│       ├── hooks/
│       │   ├── hooks.json        # Hook wiring
│       │   └── *.cjs             # Hook scripts
│       ├── skills/
│       │   ├── forge-autopilot/SKILL.md
│       │   └── forge-workflow/SKILL.md
│       ├── README.md
│       └── LICENSE
├── README.md
└── LICENSE
```

The marketplace manifest lives at `.claude-plugin/marketplace.json`. Each plugin
lives in its own directory under `plugins/`, and the manifest's `source` fields
are repo-relative paths (e.g. `./plugins/forge-shiptoday`) pointing at those
directories.

## Local development & testing

Test the marketplace from a local checkout before pushing:

```bash
git clone https://github.com/ShipToday/forge-plugin-claude-marketplace
```

```
/plugin marketplace add ./forge-plugin-claude-marketplace
/plugin install forge-shiptoday@shiptoday
```

To work on the Forge plugin in isolation, point Claude Code at its directory:

```bash
claude --plugin-dir ./forge-plugin-claude-marketplace/plugins/forge-shiptoday
```

## Adding a plugin to this marketplace

1. Create `plugins/<your-plugin>/` with a `.claude-plugin/plugin.json` manifest
   (plus any `skills/`, `agents/`, `commands/`, `hooks/`, and `.mcp.json` the
   plugin needs).
2. Add an entry to the `plugins` array in
   [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) with a
   `name`, a repo-relative `source` (`./plugins/<your-plugin>`), and a
   `description`.
3. Validate the JSON and confirm it loads with
   `/plugin marketplace add ./forge-plugin-claude-marketplace`.

## License

MIT — see [`LICENSE`](LICENSE). Each plugin is additionally covered by the
license in its own directory.
