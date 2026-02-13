# Pattern5 Skill

Teach AI coding agents to check organizational standards before writing code.

## What It Does

- Agents **search** the Pattern5 library before making architectural decisions
- Agents **apply** standards (rules), patterns (solutions), and decisions (rationale) differently based on type
- Agents **report gaps** by creating draft artifacts when no guidance exists

## Prerequisites

- A [Pattern5](https://pattern5.com) account with at least one project
- The Pattern5 MCP server configured in your agent's MCP settings with a valid API key or OAuth token

## Installation

### Claude Code

Paste this into Claude Code:

```
Install the Pattern5 skill from https://github.com/nhxhnhq/pattern5-skill into this project.
```

Or run manually:

```bash
tmp=$(mktemp -d) && git clone --depth 1 https://github.com/nhxhnhq/pattern5-skill.git "$tmp" && mkdir -p .claude/skills && cp -r "$tmp/pattern5" .claude/skills/ && rm -rf "$tmp"
```

### Cursor

Copy the rule file into your project's Cursor rules directory:

```bash
tmp=$(mktemp -d) && git clone --depth 1 https://github.com/nhxhnhq/pattern5-skill.git "$tmp" && mkdir -p .cursor/rules && cp "$tmp/cursor/pattern5-governance.mdc" .cursor/rules/ && rm -rf "$tmp"
```

The rule uses Agent Requested mode — Cursor applies it automatically when it detects architectural intent.

### GitHub Copilot

Copy the instructions file into your project's `.github` directory:

```bash
tmp=$(mktemp -d) && git clone --depth 1 https://github.com/nhxhnhq/pattern5-skill.git "$tmp" && mkdir -p .github && cp "$tmp/copilot/copilot-instructions.md" .github/ && rm -rf "$tmp"
```

If you already have a `.github/copilot-instructions.md`, append the Pattern5 content to your existing file.

### Other Agents

See the [Agent Skills Specification](https://agentskills.io/specification) for integration guidance with other MCP-compatible agents. The core governance workflow (search, apply, report gaps) works with any agent connected to the Pattern5 MCP server.

## How It Works

This repository and the MCP server serve complementary roles:

- **Instruction files** (this repo) — Tell the agent *when* to query Pattern5 and *how* to interpret results. Loaded into the agent's context automatically.
- **MCP Server** — Provides the *tools* and *data*. Handles search, retrieval, submission, and project sync.

The instructions trigger when the agent detects architectural intent (creating features, choosing approaches, setting up projects). They do not trigger for syntax questions, debugging, or general knowledge.

## Repository Structure

```
pattern5-skill/
├── pattern5/           # Claude Code skill
│   ├── SKILL.md
│   └── references/
│       └── query-strategies.md
├── cursor/             # Cursor rule
│   └── pattern5-governance.mdc
├── copilot/            # GitHub Copilot instructions
│   └── copilot-instructions.md
├── TESTING.md          # Testing guide
├── LICENSE
└── README.md
```

Each directory contains the same governance guidance adapted to the agent's native instruction format.

## Versioning

This is version **1.0.0**. The MCP server is authoritative for tool schemas and response formats. If the server's interface evolves, the instruction files tell the agent to trust the server over static content.

## License

[MIT](LICENSE)

## Links

- [Pattern5](https://pattern5.com) — Platform
- [GitHub](https://github.com/nhxhnhq/pattern5-skill) — This repository
- [Agent Skills Specification](https://agentskills.io/specification) — Skill format reference
