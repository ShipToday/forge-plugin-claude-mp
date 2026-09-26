# Forge by ShipToday

Free, AI-powered product development lifecycle automation for Claude Code.

## What it does

Ask Forge by name — include `forge` or `@forge` in your message — and it
routes your request through structured PDLC workflows:

```
> forge, implement user authentication with OAuth
> @forge fix the checkout page crash on mobile
> forge, break down the notifications feature into stories
> forge, estimate story points for PROJ-123
> @forge check status of my feature
```

Forge runs only when you ask for it. If your message only mentions Forge in
passing, Claude asks whether you meant Forge before starting anything. A
message that doesn't name Forge gets a normal response, even when it's about
planning or shipping work or mentions a ticket like `PROJ-123`.

## What's included

- **Forge MCP Server** (`.mcp.json`) — connects to the hosted Forge
  orchestration engine at `https://teams.shiptoday.ai/mcp`. Exposes
  `forge__start_workflow`, `forge__update_state`, `forge__abandon_workflow`,
  `forge__get_workflow_state`, `forge__get_workflow`, `forge__save_workflow`,
  `forge__delete_workflow`, `forge__list_skills_catalog`, and
  `forge__send_feedback`.
- **`forge-autopilot` skill** — when you ask Forge by name, routes your
  request (feature requests, bug reports, PR reviews, story breakdowns,
  status checks) to the right Forge workflow.
- **`forge-workflow` skill** — conversational management for organization
  admins to author new Forge workflows or delete existing org- or
  team-scoped overrides.
- **`forge-feedback` skill** — sends feedback to the ShipToday team from
  inside a session. It shows you the exact message first and sends only
  after you confirm.
- **Hooks** (`hooks/hooks.json`) — five hook scripts on four events
  coordinate session state:
  - `UserPromptSubmit` → `prompt-router.cjs` (continuation for active
    workflows and the snoozed-tracking wake check; it never reads your
    message to decide anything)
  - `Stop` → `stop-observer.cjs` (passive session observation and
    silent checkpoints to record engineering time)
  - `PreToolUse` → `workflow-guard.cjs` (holds tools while a question
    is waiting for you or a write is waiting for your approval, and
    stamps usage onto Forge's own calls)
  - `PostToolUse` → `workflow-tracker.cjs` (tracks workflow state
    transitions and the active step's tool allowlist) and
    `must-display.cjs` (makes sure Forge's step markers and briefs are
    shown to you)

  The hook scripts make no network calls. They keep session state in
  local files and pass instructions to Claude.

## Installation

Add the ShipToday marketplace, then install the plugin from it:

```
/plugin marketplace add ShipToday/forge-plugin-claude-mp
/plugin install forge-shiptoday@shiptoday
```

## Local development

Clone this repo and point Claude Code at the root:

```bash
git clone https://github.com/ShipToday/forge-plugin-claude
claude --plugin-dir ./forge-plugin-claude
```

## License

MIT — see `LICENSE`.
