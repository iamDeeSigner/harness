# Harness

A reference base for building **agent harnesses** for different domains,
verticals and categories.

It contains the full source of [Intent](docs/reference/UPSTREAM_README.md) by
intent-hq, a desktop app for coordinating coding agents at scale. That code is
kept intact so each new harness idea can borrow from a working, heavily tested
system:

- **Structure:** a local daemon that owns all state and runs agents, with thin
  clients on top.
- **Models:** any agent CLI that speaks ACP, the Agent Client Protocol (Claude
  Code, Codex, OpenCode, Pi, Grok, Auggie, local models), plus a live model
  catalog.
- **Tools and MCP:** a single `workspace_api` tool that runs sandboxed
  JavaScript against a `ws.*` API, plus user-configured MCP servers.
- **Doctrine:** versioned agent instructions, pinned per session.
- **Design, interaction and motion:** a token-based design system with a
  motion guide, panels and tabs, chat and streaming UI.

## Start here

| | |
|---|---|
| [Roadmap and decisions](docs/harness/ROADMAP.md) | What this repo is for, the iOS and remote-access plans, rebrand notes, known issues |
| [Docs map](docs/harness/DOCS_MAP.md) | Every doc in the repo and submodules, grouped by topic |
| [Architecture](docs/ARCHITECTURE.md) | How the daemon is built |
| [Protocol](docs/protocol/README.md) | How any client talks to the daemon |
| [Design system](https://github.com/iamDeeSigner/cloudlands-fe/blob/main/docs/DESIGN_SYSTEM.md) | Look, interaction and motion |

## Layout

| Path | Repository | What it is |
|---|---|---|
| `packages/intentd` | [iamDeeSigner/intentd](https://github.com/iamDeeSigner/intentd) | Rust daemon: agents, providers, MCP, storage, protocol |
| `packages/cloudlands-fe` | [iamDeeSigner/cloudlands-fe](https://github.com/iamDeeSigner/cloudlands-fe) | Desktop app: Electron + SvelteKit |
| `docs/` | this repo | Architecture, wire protocol, frontend guides, Harness notes |
| `scripts/`, `Makefile` | this repo | Build, test and consistency tooling |

Both packages are forks of the intent-hq originals. A future iOS app will live at
`packages/ios` ([plan](docs/harness/ROADMAP.md#ios-app)).

## Getting the code

```sh
git clone --recurse-submodules https://github.com/iamDeeSigner/harness.git
cd harness
make doctor   # lists missing prerequisites
make build    # builds the daemon
make dev      # runs the desktop app with its own daemon
```

`make help` lists every target. See [CONTRIBUTING.md](CONTRIBUTING.md) for
the full build and test setup.

## License

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE). The original
work is by The Intent Authors; keep both files in anything built from this code.
The license does not cover the Intent name or branding.
