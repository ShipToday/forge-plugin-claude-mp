# Forge by ShipToday

Free, AI-powered product development lifecycle automation for Claude Code.

## What it does

Just describe what you want to build — Forge automatically routes your request
through structured PDLC workflows. No trigger words needed.

```
> implement user authentication with OAuth
> fix the checkout page crash on mobile
> break down the notifications feature into stories
> estimate story points for PROJ-123
> check status of my feature
```

## What's included

- **Forge MCP Server** (`.mcp.json`) — connects to the hosted Forge
  orchestration engine at `https://teams.shiptoday.ai/mcp`. Exposes
  `forge__start_workflow`, `forge__update_state`, `forge__abandon_workflow`,
  `forge__get_workflow`, `forge__save_workflow`, `forge__delete_workflow`,
  and `forge__list_skills_catalog`.
- **`forge-autopilot` skill** — automatically detects product-development
  intent (feature requests, bug reports, PR reviews, story breakdowns,
  status checks, tracked work-item keys like `PROJ-123`) and routes the
  request to the right Forge workflow.
- **`forge-workflow` skill** — conversational management for organization
  admins to author new Forge workflows or delete existing org- or
  team-scoped overrides.
- **Hooks** (`hooks/hooks.json`) — four hooks coordinate session state:
  - `UserPromptSubmit` → `prompt-router.cjs` (advisory routing on epic-key
    references; continuation for active workflows)
  - `Stop` → `stop-observer.cjs` (passive session observation and
    silent checkpoints to record engineering time)
  - `PreToolUse` → `workflow-guard.cjs` (per-step tool permission
    enforcement and checkpoint gating)
  - `PostToolUse` → `workflow-tracker.cjs` (tracks workflow state
    transitions and the active step's tool allowlist)

## Installation

Once approved in the official marketplace:

```
/plugin install forge@claude-plugins-official
```

## Local development

Clone this repo and point Claude Code at the root:

```bash
git clone https://github.com/ShipToday/forge-plugin-claude
claude --plugin-dir ./forge-plugin-claude
```

## License

MIT — see `LICENSE`.
