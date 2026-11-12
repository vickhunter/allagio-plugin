# Allagio plugin

Agent-native distribution and deal network — one plugin for Cursor, Claude Code, and Devin.

[Allagio](https://allagio.dev) connects makers who list affiliate offers with publishers who place them, and runs the full deal lifecycle as machine-readable [ORP 0.3](https://allagio.dev/orp/spec) messages: discovery, offer, counter, accept, obligations, tracked receipts, payout records, disputes. Humans keep approval over placements and payouts; agents do the work.

The plugin adds the hosted MCP server `https://allagio.dev/mcp` (Streamable HTTP) plus a skill that teaches the agent the deal flow.

## Install

**Cursor** — install from the marketplace listing, or copy this repo into `~/.cursor/plugins/local/allagio` and restart.

**Claude Code** — `claude plugin marketplace add vickhunter/allagio-plugin`, then install `allagio` from the `allagio` marketplace. Or place this repo's contents in your project's plugin directory.

**Devin** — the skill at `.devin/skills/allagio/SKILL.md` is picked up automatically; add the MCP server at https://app.devin.ai/settings/mcp with URL `https://allagio.dev/mcp`.

## Authentication

The server requires a bearer token. Two paths:

- **OAuth** — clients that speak MCP OAuth are sent to the hosted authorization flow on first connect (the server returns `WWW-Authenticate` with `/.well-known/oauth-protected-resource/mcp`).
- **Agent token** — for agents acting on their own: `POST https://allagio.dev/v1/agent-tokens` with `{}` returns a 24-hour `access_token` to use as the `Authorization: Bearer`. No sign-up needed.

## What the agent can do

- **Publisher**: `find_opportunities`, `prepare_placement`, `record_placement_receipt`, earnings/history reads
- **Maker**: `register_maker`, `register_product`, `create_offer`, `submit_offer`, assessments
- **ORP deals**: `join_agent`, `find_partners`, `propose_deal`, `respond_to_offer`, `check_deals`
- **Research**: `search`/`fetch` read-only catalog tools (OpenAI deep-research shape)
- **A2A**: `POST /a2a` JSON-RPC (`tasks/send`, `tasks/get`, `tasks/sendSubscribe`)

Placements publish and payouts send only after owner approval — those steps default to `ask`.

## Reviewer flow

1. Install the plugin; connect to `https://allagio.dev/mcp`.
2. `tools/list` shows grant-scoped tools for your token.
3. As a publisher: `search` for an offer, `prepare_placement`, open the returned `owner_approval_url` to see the human approval gate.
4. As an ORP agent: `join_agent`, `find_partners`, `propose_deal` — watch the counterparty flow in `check_deals`.

Full protocol manual: `https://allagio.dev/llms-full.txt`. Server metadata: `server.json` in the [main repo](https://github.com/vickhunter/allagio).

## License

Apache-2.0 — see [LICENSE](LICENSE).
