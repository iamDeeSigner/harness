# Docs Map

Every piece of documentation in this repo and its two submodules, grouped by
what you'd go to it for. Each entry has its line count so you know what you're
opening.

Links into `packages/intentd` and `packages/cloudlands-fe` point at the forks
on GitHub ([iamDeeSigner/intentd](https://github.com/iamDeeSigner/intentd),
[iamDeeSigner/cloudlands-fe](https://github.com/iamDeeSigner/cloudlands-fe)),
so they work both on GitHub and locally. Locally, the same files live under
`packages/<name>/`.

Snapshot taken 2026-10-10: monorepo at `fbf7333`, intentd at `5c7b55c`
(v0.10.42), cloudlands-fe at `3131716`.

**Reading order for a new harness idea:** [Roadmap](./ROADMAP.md) →
[Architecture](../ARCHITECTURE.md) → [Harness versioning](../HARNESS.md) →
[Adding a provider](https://github.com/iamDeeSigner/intentd/blob/main/crates/intent-providers/ADDING_A_PROVIDER.md)
→ [MCP bindings](../protocol/methods/mcp-bindings.md) →
[Design system](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/docs/DESIGN_SYSTEM.md).

---

## 1. Your notes (Harness)

| Doc | What it's for |
|---|---|
| [ROADMAP.md](./ROADMAP.md) | Plans and decisions: iOS app, remote access, the rebrand, vertical packs, known issues |
| [DOCS_MAP.md](./DOCS_MAP.md) | This file |
| [UPSTREAM_README.md](../reference/UPSTREAM_README.md) | The original Intent README, archived |

## 2. Big picture and architecture

| Doc | Lines | What it's for |
|---|---|---|
| [ARCHITECTURE.md](../ARCHITECTURE.md) | 1378 | The backend design: crates, dependency rules, SQLite persistence, transports, ACP agent runtime, completion settlement. **The most important doc.** |
| [HARNESS.md](../HARNESS.md) | 173 | How agent instructions ("doctrine") are versioned and pinned per session. Note: says the current version is 2.7, but the code is at 3.1 |
| [docs/README.md](../README.md) | 135 | The upstream index of the `docs/` tree |
| [intentd README](https://github.com/iamDeeSigner/intentd/blob/main/README.md) | 853 | The daemon: what it owns, CLI, config, data directories |
| [cloudlands-fe README](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/README.md) | 243 | The desktop app: Electron + SvelteKit layout and how it talks to the daemon |

## 3. Agent harness core: prompts, doctrine, roles

This is the part you'll rewrite most for each vertical: the words that shape
how agents behave.

| Doc | Lines | What it's for |
|---|---|---|
| [HARNESS.md](../HARNESS.md) | 173 | Versioning rules, golden tests, how to mint a new doctrine version |
| [Agent instructions (all versions)](https://github.com/iamDeeSigner/intentd/tree/main/crates/intent-services/resources/agent-instructions) | — | The bundled system-prompt markdown. `v1/` has the full set of 17 roles (`common.md`, `workspace.md`, `chat.md`, `task-loop.md`, `ralph-loop.md`, `code-review.md`, `commit-message.md`, `pr-description.md`, `debug.md`, `task-breakdown.md`, …). Later versions (v1.1 … v3.1) override only what changed |
| [Specialists (daemon)](https://github.com/iamDeeSigner/intentd/tree/main/crates/intent-services/resources/specialists) | — | Persona prompts for delegated agents: chief-of-staff, developer, implementor, pr-reviewer, ralph, spec-writer, ui-designer, verifier |
| [Specialists (desktop copy)](https://github.com/iamDeeSigner/cloudlands-fe/tree/main/resources/specialists) | — | The same personas as shipped with the desktop app, plus vulnerability-scanner |
| [assistant-app-guide.md](https://github.com/iamDeeSigner/intentd/blob/main/crates/intent-services/resources/assistant-app-guide.md) | 351 | The in-app user guide the built-in assistant reads to help users |
| [RULES_SYSTEM.md](../fe/RULES_SYSTEM.md) | 173 | How user, workspace and built-in rules are layered into prompts |
| [tasks-block-syntax.md](../fe/tasks-block-syntax.md) | 179 | The task syntax agents are taught to write, and common mistakes |
| [hooks.md](../protocol/methods/hooks.md) | 285 | Background hooks: small JS scripts agents schedule to wake themselves later |

## 4. Models and providers

How the harness plugs into Claude Code, Codex, OpenCode, Pi, local models and others.

| Doc | Lines | What it's for |
|---|---|---|
| [ADDING_A_PROVIDER.md](https://github.com/iamDeeSigner/intentd/blob/main/crates/intent-providers/ADDING_A_PROVIDER.md) | 448 | Step-by-step checklist for wiring in a new agent CLI over ACP. **Key doc for model compatibility** |
| [models-providers.md](../protocol/methods/models-providers.md) | 313 | `models.list` and the model catalog wire contract |
| [ACP implementation (desktop)](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/features/acp-official/README.md) | 192 | The Agent Client Protocol as implemented in the frontend |
| [agent-aux.md](../protocol/methods/agent-aux.md) | 626 | Session stats, token usage and other per-agent extras |
| [agent-workers.md](../protocol/methods/agent-workers.md) | 253 | Read-only Codex worker observations (prepared, not built) |
| [model-platform-routing.md](../protocol/model-platform-routing.md) | 390 | Placing agents on machines by OS and CPU type. Despite the name, not about picking LLMs (prepared, not built) |

## 5. Tools and MCP

| Doc | Lines | What it's for |
|---|---|---|
| [mcp-bindings.md](../protocol/methods/mcp-bindings.md) | 288 | Every `ws.*` function agents can call through the single `workspace_api` tool. **The tool surface in one page** |
| [mcp-servers.md](../protocol/methods/mcp-servers.md) | 274 | User-configured external MCP servers: config, transports, testing, forwarding |
| [MCP system (desktop)](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/features/mcp/README.md) | 239 | How the frontend side of MCP is structured |
| [CDP_MCP_TOOLS.md](../fe/CDP_MCP_TOOLS.md) | 207 | Tools that let agents inspect and drive the running app via Chrome DevTools |
| [Browser API overview](https://github.com/iamDeeSigner/intentd/blob/main/crates/intent-acp/src/mcp_server/bindings/browser_docs/overview.md) | 285 | The agent-facing browser API (embedded tabs) |
| [Browser API examples](https://github.com/iamDeeSigner/intentd/blob/main/crates/intent-acp/src/mcp_server/bindings/browser_docs/examples.md) | 475 | Worked examples of agents using the browser |
| [Browser capture](https://github.com/iamDeeSigner/intentd/blob/main/crates/intent-acp/src/mcp_server/bindings/browser_docs/capture.md) | 101 | Network and console capture for debugging |
| [scripts.md](../protocol/methods/scripts.md) | 768 | Saved scripts and long-running services agents can start |
| [script-monitors.md](../protocol/methods/script-monitors.md) | 415 | Watching script output for patterns (prepared, not built) |
| [08-permission-flow.md](../protocol/08-permission-flow.md) | 145 | How tool-permission prompts reach the user and get answered |

## 6. Wire protocol (how any client talks to the daemon)

Read in order. This is what a CLI, web or mobile client is built against.

| Doc | Lines | What it's for |
|---|---|---|
| [protocol/README.md](../protocol/README.md) | 338 | Protocol overview, version (13.8) and section map |
| [01-transport.md](../protocol/01-transport.md) | 123 | Unix socket and HTTPS/WebSocket listeners, ports, TLS |
| [02-authentication.md](../protocol/02-authentication.md) | 61 | Bearer tokens and origin checks on connect |
| [03-envelope.md](../protocol/03-envelope.md) | 60 | JSON-RPC 2.0 message rules |
| [04-heartbeat.md](../protocol/04-heartbeat.md) | 9 | Keep-alive and lifecycle |
| [05-method-catalog.md](../protocol/05-method-catalog.md) | 806 | Every method, with links into `methods/` |
| [06-events.md](../protocol/06-events.md) | 760 | Live event subscriptions, the core of keeping a client in sync |
| [07-agent-streaming.md](../protocol/07-agent-streaming.md) | 916 | How agent output streams to clients, interruptions, abnormal endings |
| [08-permission-flow.md](../protocol/08-permission-flow.md) | 145 | Permission prompts |
| [09-error-codes.md](../protocol/09-error-codes.md) | 106 | Error codes |
| [10-thin-client.md](../protocol/10-thin-client.md) | 332 | Rules for building a client that holds no state of its own. **Start here for any new client** |
| [versioning.md](../protocol/versioning.md) | 559 | Version history and compatibility policy |
| [workspace-routing.md](../protocol/workspace-routing.md) | 252 | Routing calls to the right workspace (prepared) |
| [event-types.json](../protocol/event-types.json) | — | Machine-readable list of every event type |
| [fixtures/](../protocol/fixtures/) | — | Contract test fixtures. Any client you build should pass these |
| [PROTOCOL.md](../PROTOCOL.md) | 14 | Redirect stub only |

## 7. Mobile and remote access (future iOS app)

Everything the future iOS app and "use it from any network" work will lean on.
See [Roadmap → iOS](./ROADMAP.md#ios-app) and
[Roadmap → Remote access](./ROADMAP.md#remote-access-from-any-network).

| Doc | Lines | What it's for |
|---|---|---|
| [10-thin-client.md](../protocol/10-thin-client.md) | 332 | Client rules, including iOS pairing behaviour |
| [01-transport.md](../protocol/01-transport.md) / [02-authentication.md](../protocol/02-authentication.md) | 184 | How a phone connects and authenticates today (same network only) |
| [backend-keychain.md](../protocol/backend-keychain.md) | 194 | How desktop and iOS share saved daemon credentials in Apple Keychain, with a fixture corpus |
| [client-hello.md](../protocol/methods/client-hello.md) | 412 | The handshake every client sends, and stable client identity |
| [MULTI_BACKEND_CONNECT.md](../fe/MULTI_BACKEND_CONNECT.md) | 216 | One client connected to several daemons at once |
| [multiplayer.md](../protocol/methods/multiplayer.md) | 279 | People, invites and membership |
| [shared-host-membership.md](../protocol/methods/shared-host-membership.md) | 1132 | Several people sharing one daemon |
| [presence.md](../protocol/methods/presence.md) | 160 | Who's here right now |
| [execution-locus.md](../protocol/methods/execution-locus.md) | 256 | Where work runs: local versus remote |
| [nodes.md](../protocol/methods/nodes.md) | 276 | Running agents on other machines (prepared, not built) |
| [node-link.md](../protocol/node-link.md) | 1709 | The private link between daemon and remote machines (prepared). **Most relevant design for remote access** |
| [node-checkpoints.md](../protocol/node-checkpoints.md) | 560 | Hub and checkpoint design for remote machines (prepared) |

## 8. Design, interaction and motion

The look and feel to study and borrow from.

| Doc | Lines | What it's for |
|---|---|---|
| [DESIGN_SYSTEM.md](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/docs/DESIGN_SYSTEM.md) | 726 | **The design system.** Task-first ("I need to notify the user…"), semantic colour roles, non-colour scales, theme boundary, **Motion and interaction** (§598), size ladder, audits |
| [DESIGN_SYSTEM_CHEATSHEET.md](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/docs/DESIGN_SYSTEM_CHEATSHEET.md) | 314 | Quick pattern lookup, with a live component catalogue (`pnpm run dev:ui`) |
| [tokens.css](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/lib/styles/tokens.css) | 757 | **The design tokens** (~570): colour, type, spacing, radius, motion, light and dark. The source for any iOS port |
| [Other styles](https://github.com/iamDeeSigner/cloudlands-fe/tree/main/src/lib/styles) | — | Chat messages, comments, overlays, resize handles, editor, syntax highlighting |
| [Design-system lint rules](https://github.com/iamDeeSigner/cloudlands-fe/tree/main/eslint-rules/design-system) | — | Rules that enforce the system in code (no raw colours, no ad-hoc transitions, no raw controls…) |
| [COMPONENTS_DESIGN.md](../fe/COMPONENTS_DESIGN.md) | 156 | How to structure and size components |
| [PANEL_TAB_UX_SPEC.md](../fe/PANEL_TAB_UX_SPEC.md) | 407 | Panels and tabs: the main layout interaction model |
| [KEYBINDINGS.md](../fe/KEYBINDINGS.md) | 207 | Every keyboard shortcut |
| [BROWSER_PANEL_SPEC.md](../fe/BROWSER_PANEL_SPEC.md) | 299 | The embedded browser panel |
| [code-review-ui.md](../fe/code-review-ui.md) | 288 | The AI code-review interface |
| [Diagram primitives](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/lib/components/diagrams/README.md) | 154 | Interactive diagrams agents and users can create |
| [List components](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/lib/components/ui/list/README.md) | 156 | Sidebar list patterns |
| [Mention system](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/lib/services/mentions/README.md) | 1221 | `@` mentions for files, notes, tasks and more |
| [workspaces-link-handler.md](../fe/workspaces-link-handler.md) | 214 | In-app `intent://` links |

## 9. Frontend engineering

| Doc | Lines | What it's for |
|---|---|---|
| [DEVELOPER_GUIDE.md](../fe/DEVELOPER_GUIDE.md) | 489 | Repo layout, APIs, remote sandboxes |
| [STATE_MANAGEMENT.md](../fe/STATE_MANAGEMENT.md) | 84 | State management with the Themis runtime |
| [Redux store directives](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/store/renderer/AGENTS.md) | 148 | Store rules, plus [architecture](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/store/renderer/docs/REDUX_ARCHITECTURE_GUIDE.md), [reducers](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/store/renderer/docs/REDUCERS_GUIDE.md), [selectors](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/store/renderer/docs/SELECTORS_GUIDE.md) and [migration](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/src/store/renderer/docs/MIGRATION_GUIDE.md) guides |
| [EVENT_SYSTEM.md](../fe/EVENT_SYSTEM.md) | 273 | Frontend event architecture |
| [agent-message-dedup-and-stream-sagas.md](../fe/agent-message-dedup-and-stream-sagas.md) | 134 | Avoiding duplicate messages while streaming |
| [ERROR_HANDLING_SYSTEM.md](../fe/ERROR_HANDLING_SYSTEM.md) | 273 | Error handling |
| [TYPE_SYSTEM_GUIDE.md](../fe/TYPE_SYSTEM_GUIDE.md) | 203 | Keeping backend and frontend types in step |
| [MODULE_BOUNDARY_GUIDE.md](../fe/MODULE_BOUNDARY_GUIDE.md) | 124 | Where helper modules go |
| [IPC_DEBUG_GUIDE.md](../fe/IPC_DEBUG_GUIDE.md) | 188 | Debugging Electron IPC |
| [panel-system-refactoring.md](../fe/panel-system-refactoring.md) | 141 | History of the panel refactor |
| [TROUBLESHOOTING_GUIDE.md](../fe/TROUBLESHOOTING_GUIDE.md) | 331 | Common problems and fixes |

## 10. Domain features (coding-specific today)

The workspace features built for software teams. For a new vertical, these are
what you replace or reshape; the platform underneath stays.

| Doc | Lines | Domain |
|---|---|---|
| [workspace.md](../protocol/methods/workspace.md) | 2961 | Workspaces (the core unit of work) |
| [agents.md](../protocol/methods/agents.md) | 2148 | Agents: create, message, delegate, harness stamps (§5.5) |
| [notes-tasks.md](../protocol/methods/notes-tasks.md) | 594 | Notes and tasks |
| [files-terminal-browser.md](../protocol/methods/files-terminal-browser.md) | 950 | Files, terminals, browser tabs |
| [git.md](../protocol/methods/git.md) | 152 | Git |
| [pr.md](../protocol/methods/pr.md) | 482 | Pull requests |
| [change-tracking.md](../protocol/methods/change-tracking.md) | 475 | Reviewing and accepting agent changes |
| [repository-checkout.md](../protocol/methods/repository-checkout.md) / [repository-resources.md](../protocol/methods/repository-resources.md) | 641 | GitLab checkout and resource reads (prepared) |
| [integrations.md](../protocol/methods/integrations.md) | 1063 | GitHub, Linear, Sentry, and future integrations |
| [search-drafts.md](../protocol/methods/search-drafts.md) | 280 | Search and drafts |
| [voice.md](../protocol/methods/voice.md) | 199 | Speech-to-text |
| [settings.md](../protocol/methods/settings.md) | 554 | Settings, including `agentFeatures` toggles |
| [desktop.md](../protocol/methods/desktop.md) | 801 | Desktop control (prepared) |
| [misc-namespaces.md](../protocol/methods/misc-namespaces.md) | 476 | Cross-workspace, primitives, specialists, repos |
| [events-query.md](../protocol/methods/events-query.md) | 67 | Historical event queries |
| [system-observability.md](../protocol/methods/system-observability.md) | 163 | Build identity and health |

## 11. Process, release and agent workflow

| Doc | Lines | What it's for |
|---|---|---|
| [AGENTS.md (root)](../../AGENTS.md) | 452 | How AI agents should work in this repo. Upstream workflow; mostly intent-hq specific |
| [intentd AGENTS.md](https://github.com/iamDeeSigner/intentd/blob/main/AGENTS.md) | 291 | Daemon-specific agent rules: gates, test policy |
| [cloudlands-fe AGENTS.md](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/AGENTS.md) | 770 | Frontend routing guide for agents |
| [CONTRIBUTING.md](../../CONTRIBUTING.md) | 363 | Building, testing, local Rust gates |
| [SECURITY.md](../../SECURITY.md) | 34 | Upstream security policy |
| [RELEASING.md](../RELEASING.md) / [fe/RELEASING.md](../fe/RELEASING.md) / [DEPLOYING.md](../fe/DEPLOYING.md) | 1260 | Upstream release pipeline. Points at intent-hq infrastructure |
| [PR_DESCRIPTION_GUIDE.md](../fe/PR_DESCRIPTION_GUIDE.md) | 87 | How PRs are written |
| [isolated-test-builds.md](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/docs/isolated-test-builds.md) | 54 | Isolated macOS test builds |
| [repo-retrospective skill](../../.agents/skills/repo-retrospective/SKILL.md) | 127 | End-of-work retrospective prompt |
| [Frontend agent skills](https://github.com/iamDeeSigner/cloudlands-fe/tree/main/.agents/skills) | — | Electron automation, PR description, redux-saga, and Themis state-library skills |
| [review-response.md](https://github.com/iamDeeSigner/intentd/blob/main/review-response.md) | 45 | A leftover review note |
