# Testing the Pattern5 Skill

## Verifying Installation

### Claude Code

Ask the agent:

> "When would you use the pattern5 skill?"

The agent should respond with a summary matching the skill's description: checking organizational governance before architectural decisions, creating features, choosing approaches, etc. If the agent does not recognize the skill, verify `pattern5/SKILL.md` is in `.claude/skills/pattern5/`.

### Cursor

Open a Cursor chat and ask:

> "What rules do you have about Pattern5?"

Cursor should reference the governance rule. If not, verify `pattern5-governance.mdc` is in `.cursor/rules/`.

### GitHub Copilot

Open Copilot Chat and ask:

> "What instructions do you have about Pattern5?"

Copilot should reference the governance instructions. If not, verify `copilot-instructions.md` is in `.github/`.

## Should-Trigger Queries

The following prompts should cause the agent to invoke Pattern5 MCP tools:

1. "Add user authentication to the app"
2. "Create a new API endpoint for orders"
3. "Set up the database schema for this project"
4. "Refactor the payment processing module"
5. "Add caching to the product listing page"
6. "Implement role-based access control"
7. "Choose between REST and GraphQL for the new API"
8. "Add a notification system"
9. "Set up error handling for server actions"
10. "Onboard me to this codebase"

## Should-NOT-Trigger Queries

The following prompts should NOT invoke Pattern5 tools:

1. "What does the `map` function do in JavaScript?"
2. "Fix the TypeError on line 42"
3. "How do I install Node.js?"
4. "Explain this regex pattern"
5. "What's the difference between `let` and `const`?"
6. "Debug why my tests are failing"
7. "How do I use the `fetch` API?"
8. "What does this error message mean?"

## Functional Verification

After confirming trigger behavior, verify end-to-end functionality. These checks apply to all agents.

### Search-Before-Build

1. Give the agent an implementation task (e.g., "Add form validation to the signup page").
2. Verify the agent calls `pattern5_search` before writing implementation code.
3. If results are returned, verify the agent calls `pattern5_get` to retrieve full details.

### Type Differentiation

1. Verify the agent treats standards as rules (checks enforcement level, follows compliant examples).
2. Verify the agent treats patterns as guidance (checks `apply_when`, follows `structure`).
3. Verify the agent treats decisions as context (checks `decision_status`, reads `rationale`).

### Gap Reporting

1. Give the agent a task in an area with no existing artifacts.
2. Verify the agent calls `pattern5_submit_pattern`, `pattern5_submit_standard`, or `pattern5_submit_decision` to create a draft.

### Rating

1. After the agent applies an artifact, verify it calls `pattern5_rate`.

## Performance Comparison

To measure the skill's impact:

1. Complete a task (e.g., "Add a new API endpoint") **without** the instruction files installed.
2. Complete the same task **with** the instruction files installed and the MCP server connected.
3. Compare whether the agent checked for organizational standards and followed existing patterns.

## Agent-Specific Notes

### Claude Code

- The skill uses **auto-discovery**: Claude Code scans `.claude/skills/` for `SKILL.md` files and loads metadata automatically.
- The skill triggers based on the `description` field in SKILL.md frontmatter.
- Reference files in `references/` are loaded on demand, not upfront.

### Cursor

- The rule uses **Agent Requested** mode (`alwaysApply: false`): Cursor reads the `description` field and decides whether to apply the rule based on the current task.
- To force-apply, mention `@pattern5-governance` in the chat.

### GitHub Copilot

- The instructions file is **always loaded** when Copilot is active in the repository.
- Copilot does not have conditional triggering — the instructions are always in context.
- Keep the file concise since it counts toward Copilot's context budget on every interaction.

## Iteration Guidance

### Under-Triggering

If the agent does not activate on queries where it should:

- **Claude Code**: Add more trigger phrases to the `description` field in SKILL.md frontmatter.
- **Cursor**: Expand the `description` field in the `.mdc` frontmatter.
- **Copilot**: Add more explicit "when to query" guidance to the instructions file.

### Over-Triggering

If the agent activates on queries where it should not:

- **Claude Code / Cursor**: Add more specific negative conditions to the description.
- **Copilot**: Expand the "When NOT to Query" section with more examples.
