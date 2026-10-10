# Harness Roadmap and Decisions

Living notes for this repo: what it is for, what has been decided, and what is
planned but not started. Update it whenever a decision changes.

Started 2026-10-10.

## What this repo is

**Harness** is a reference base for building agent harnesses for different
domains, verticals and categories. It starts as a snapshot of
[Intent](../reference/UPSTREAM_README.md) by intent-hq, a desktop app for
coordinating coding agents, and keeps its full source, so each new harness idea
can borrow its structure, features, design, interaction and motion.

- **The upstream code stays intact here.** Treat this repo as the library you
  pull from, not the place a specific product is built. Each new harness gets
  its own repo (or a branch), and takes what it needs from here.
- **License:** Apache-2.0. Keep `LICENSE` and `NOTICE` in anything derived from
  this code. The license covers the code, not the Intent name or branding.

### Sources

| Component | Path | Your fork | Upstream | Pinned at |
|---|---|---|---|---|
| Monorepo (docs, tooling, CI) | `/` | this repo | [intent-hq/intent](https://github.com/intent-hq/intent) | `fbf7333` |
| Rust daemon | `packages/intentd` | [iamDeeSigner/intentd](https://github.com/iamDeeSigner/intentd) | [intent-hq/intentd](https://github.com/intent-hq/intentd) | `5c7b55c` (v0.10.42) |
| Desktop app | `packages/cloudlands-fe` | [iamDeeSigner/cloudlands-fe](https://github.com/iamDeeSigner/cloudlands-fe) | [intent-hq/cloudlands-fe](https://github.com/intent-hq/cloudlands-fe) | `3131716` |
| iOS app | *(removed)* | none | intent-hq/ios (private) | — |

To pull later upstream improvements into a fork, use **Sync fork** on the
fork's GitHub page, then advance the pin here.

## How it works, in one paragraph

`intentd` is a local Rust daemon that holds all state (SQLite) and runs the
agents. It does not call LLM APIs itself: it launches existing agent CLIs
(Claude Code, Codex, OpenCode, Pi, Grok, Auggie, local models through Unsloth)
and talks to them over ACP, the Agent Client Protocol. Each agent gets one MCP
tool, `workspace_api`, through which it runs sandboxed JavaScript against a
`ws.*` API: the same backend the desktop app uses. All the text that shapes
agent behaviour is versioned and pinned per session. Clients (desktop, future
mobile) talk to the daemon over JSON-RPC. Full map: [DOCS_MAP.md](./DOCS_MAP.md).

## Decisions

| Date | Decision |
|---|---|
| 2026-10-10 | Repo renamed in spirit to **Harness**: a reference base for many harness ideas, not one product |
| 2026-10-10 | Forked `intentd` and `cloudlands-fe` to iamDeeSigner; submodules point at the forks |
| 2026-10-10 | Removed the private `packages/ios` pointer. Nobody outside intent-hq can clone it. An iOS app will be built fresh, later |
| 2026-10-10 | Future builds must work from **any network**, not only the same Wi-Fi as the daemon |

---

## iOS app

**Status: planned, not started.** Development happens on a Mac with Xcode.

The original iOS app is private, so none of its code is available. A new
SwiftUI app will be built from scratch and slot in at `packages/ios` as its own
repo. Everything it needs to match is public in this repo:

- **Behaviour, from the protocol docs.** Pairing, authentication, streaming,
  permissions and keychain storage are all specified (see
  [DOCS_MAP → Mobile and remote access](./DOCS_MAP.md#7-mobile-and-remote-access-future-ios-app)).
  The fixtures in [`docs/protocol/fixtures/`](../protocol/fixtures/), especially
  `backend-keychain/`, become the app's conformance tests.
- **Look, from the design system.** The ~570 tokens in
  [`tokens.css`](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/lib/styles/tokens.css),
  the [design system guide](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/docs/DESIGN_SYSTEM.md)
  (including its Motion and interaction section) and the
  [component guidelines](../fe/COMPONENTS_DESIGN.md).

### Planned structure

1. **Shared token pipeline.** A script converts `tokens.css` into Swift
   (colours, type, spacing, radius, motion; light and dark). One source of
   truth, so desktop and iOS never drift.
2. **Protocol layer** as a separate Swift package, written against the docs and
   tested against the fixtures. It must not depend on the UI, so it survives
   every vertical's redesign.
3. **SwiftUI app** on top: components that mirror the desktop ones (chat,
   agent cards, permission sheets, workspace lists), native in feel, the same
   product in look.

### MVP scope

Pair with a daemon → see workspaces and agents → chat with an agent with live
streaming → approve or deny permission requests → get notified when an agent
needs attention.

### Open questions

- iOS only, or cross-platform? The original is SwiftUI, iPhone only. Android
  matters a lot in some markets; a cross-platform app or a mobile web client
  would reach more people.
- Could a phone-friendly web build of the desktop app (it is SvelteKit) serve
  as the first mobile client before going native?

## Remote access from any network

**Status: requirement, not designed.**

**Today:** phones and other clients reach the daemon over WebSocket/TLS on the
local network only ([01-transport.md](../protocol/01-transport.md),
[02-authentication.md](../protocol/02-authentication.md)). Users must be on the
same Wi-Fi as the machine running the daemon.

**Requirement:** future builds must work across networks: phone on mobile data,
laptop on another Wi-Fi, a teammate in another city.

### Options to evaluate

| Option | How it works | Trade-offs |
|---|---|---|
| **Relay service** | Daemon and clients both connect out to a relay you host; the relay forwards encrypted traffic | Works through any network and firewall. You run and pay for infrastructure. Traffic should be end-to-end encrypted so the relay can't read it |
| **Mesh VPN** (Tailscale/WireGuard style) | Devices join a private network and reach the daemon as if local | Very little code to write. Every user must install and set up the VPN, which is a hard sell outside developer audiences |
| **Hosted daemon** | Run `intentd` in the cloud per user or team; clients connect to it over the internet | Simplest for end users, always on. Changes the product from local-first to hosted: data location, cost, multi-tenancy, security all become your responsibility |
| **Hybrid** | Local daemon by default, with an optional relay or hosted mode | Most flexible. Most work |

### Prior art already in the docs

- [node-link.md](../protocol/node-link.md): a private, authenticated link
  between a daemon and remote machines (prepared, not built). The closest
  existing design to what remote access needs.
- [nodes.md](../protocol/methods/nodes.md),
  [node-checkpoints.md](../protocol/node-checkpoints.md),
  [execution-locus.md](../protocol/methods/execution-locus.md): running work
  on other machines.
- [MULTI_BACKEND_CONNECT.md](../fe/MULTI_BACKEND_CONNECT.md),
  [shared-host-membership.md](../protocol/methods/shared-host-membership.md),
  [multiplayer.md](../protocol/methods/multiplayer.md): one client and many
  daemons, many people on one daemon.

### Must fix before exposing anything beyond the local machine

See [Known issues](#known-issues-found-in-review). The unauthenticated local
tool-bridge port and the allow-all permission default are acceptable on a
single-user laptop and not acceptable on anything reachable from outside.

## Rebrand to Harness

**Done:** the repo README and these notes.

**Not done, on purpose:** the code still says Intent, `intentd`,
`cloudlands-fe`, the `intent://` link scheme, Apple keychain IDs
(`com.cloudlands.intent.*`), bundle IDs, and the release and update URLs that
point at intent-hq. Renaming all of that inside the reference copy would make
it harder to compare with or sync from upstream. Do the full rename in each
new harness built from this repo, with its own name.

Checklist for a new harness's rename:

- [ ] Product name, app name, window titles, icons
- [ ] Binary and crate names (`intentd`, `intent-*`)
- [ ] Link scheme (`intent://`) and keychain or bundle identifiers
- [ ] Update and release URLs (desktop auto-update, sitter self-update): point at your own releases or remove
- [ ] Data directories and socket paths (`.../intentd/`)
- [ ] User-facing strings in `cloudlands-fe/messages`
- [ ] Keep `LICENSE` and `NOTICE`; add your own copyright line

## Repurposing for other verticals

**Status: idea.** First design pass to do when the first harness idea is picked.

The split to aim for: a domain-agnostic **platform**, plus a swappable
**vertical pack** per domain.

| Platform (keep) | Coding-specific (replace per vertical) |
|---|---|
| Daemon, SQLite store, event bus, JSON-RPC protocol | Git worktrees, PRs, change tracking, repo checkout |
| ACP provider layer and model catalog | GitHub, Linear and Sentry integrations |
| `workspace_api` tool and the sandboxed JS engine | Coding doctrine (`code-review.md`, `commit-message.md`, `pr-description.md`, …) |
| Doctrine versioning and per-session pinning | Specialists: developer, pr-reviewer, implementor |
| Agents, delegation, hooks, queues, permissions | Terminal and setup-script features |
| Notes, tasks, attention requests | |
| Design system, panels, chat, motion | |

A vertical pack would bundle: doctrine and specialists for the domain, the
`ws.*` namespaces the domain needs, its integrations, and its UI panels.

## Known issues found in review

Checked against the source on 2026-10-10. Fix them in any harness built from
this code, especially before remote access.

| Severity | Issue | Where |
|---|---|---|
| High | The local port that connects agents to the daemon's tools has no authentication. Any local process that finds it can act as that agent, including running shell commands | `intentd/crates/intent-acp/src/mcp_bridge.rs` (`serve_mcp_tcp_with_timeout` binds `127.0.0.1:0`) |
| High | Tool permissions default to allow-all unless `INTENTD_PERMISSION_POLICY` is set | `intentd/crates/intentd/src/main.rs` (`parse_permission_policy`) |
| Medium | MCP servers started through `ws.mcp.*` inherit the daemon's full environment, so API keys can leak to them. The other path filters secrets | `intentd/crates/intent-services/src/mcp_servers.rs` (`spawn_stdio`, no `env_clear`) |
| Medium | Turning off `agentFeatures.mcpTools` blocks `ws.mcp.*` but not the agent CLI's own MCP connections | `agent_manager.rs` (`merge_user_mcp_servers`) |
| Medium | A failed model switch on non-Codex providers only logs a warning; the agent silently runs on its default model | `agent_manager.rs` (`maybe_apply_session_model`) |
| Low | `ws.mcp.callTool` accepts up to 55 s timeouts, but the JS sandbox stops scripts at 30 s | `mcp_servers.rs` vs `intent-js/src/lib.rs` |
| Low | MCP over SSE is a connectivity check only; no MCP resources, prompts or sampling; protocol versions hardcoded | `mcp_servers.rs`, `mcp_server.rs` |
| Low | HTTP MCP tool calls redo the full handshake on every call (four round trips) | `mcp_servers.rs` |
| Maintenance | Very large files (`agent_manager.rs` ~22.7k lines, `intent-services/src/lib.rs` ~38.7k) and provider IDs hardcoded across the code despite the data-driven registry | `intent-services` |
| Docs | `HARNESS.md` says version 2.7; code is at 3.1 (`intent-core/src/model.rs`) | `docs/HARNESS.md` |

## Housekeeping

- **CI and scheduled workflows** in `.github/workflows/` still reference intent-hq
  repos and secrets that this repo doesn't have. Most skip safely without their
  secrets, but they will still run on schedule (the submodule auto-bump runs every
  30 minutes). Consider disabling the ones you don't need under
  **Actions → workflow → Disable**.
- **Makefile iOS targets** (`ios-open`, `ios-build-*`, `ios-test`,
  `ensure-ios-submodule`) no longer have a submodule to act on. Leave them until
  the new iOS app exists, then point them at it.
- **`AGENTS.md`** at the root is the upstream team's workflow (merge queues,
  intent-hq issue tracker, release monitoring). Rewrite it for your own way of
  working when you start building here.
