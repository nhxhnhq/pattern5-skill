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

Copy the `pattern5/` folder into your project's skills directory:

```bash
cp -r pattern5/ /path/to/your/project/.claude/skills/pattern5
```

Or, if using a Claude Code plugin, copy it into the plugin's `skills/` directory:

```bash
cp -r pattern5/ /path/to/plugin/skills/pattern5
```

### Other Agents

See the [Agent Skills Specification](https://agentskills.io/specification) for integration guidance with other MCP-compatible agents.

## How It Works

This skill and the MCP server serve complementary roles:

- **Skill** (SKILL.md) -- Tells the agent *when* to query and *how* to interpret results. Loaded into the agent's context when triggered.
- **MCP Server** -- Provides the *tools* and *data*. Handles search, retrieval, submission, and project sync.

The skill triggers when the agent detects architectural intent (creating features, choosing approaches, setting up projects). It does not trigger for syntax questions, debugging, or general knowledge.

## Versioning

This is version **1.0.0**. The MCP server is authoritative for tool schemas and response formats. If the server's interface evolves, the skill instructs the agent to trust the server over these static instructions.

## License

[MIT](LICENSE)

## Links

- [Pattern5](https://pattern5.com) -- Platform
- [GitHub](https://github.com/nhxhnhq/pattern5-skill) -- This repository
- [Agent Skills Specification](https://agentskills.io/specification) -- Skill format reference
