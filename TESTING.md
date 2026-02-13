# Testing the Pattern5 Skill

## Verifying Skill Loading

To confirm the skill is installed and discoverable, ask the agent:

> "When would you use the pattern5 skill?"

The agent should respond with a summary matching the skill's description: checking organizational governance before architectural decisions, creating features, choosing approaches, etc. If the agent does not recognize the skill, verify the `pattern5/SKILL.md` file is in the correct directory.

## Should-Trigger Queries

The following prompts should cause the agent to invoke the skill and call Pattern5 MCP tools:

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

The following prompts should NOT invoke the skill:

1. "What does the `map` function do in JavaScript?"
2. "Fix the TypeError on line 42"
3. "How do I install Node.js?"
4. "Explain this regex pattern"
5. "What's the difference between `let` and `const`?"
6. "Debug why my tests are failing"
7. "How do I use the `fetch` API?"
8. "What does this error message mean?"

## Functional Verification

After confirming trigger behavior, verify end-to-end functionality:

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

1. Complete a task (e.g., "Add a new API endpoint") **without** the skill installed.
2. Complete the same task **with** the skill installed and the MCP server connected.
3. Compare whether the skill-enabled agent checked for organizational standards and followed existing patterns.

## Iteration Guidance

### Under-Triggering

If the skill does not activate on queries where it should:

- Add more trigger phrases to the `description` field in SKILL.md frontmatter.
- Use phrases that match how users naturally describe the task.
- Ensure trigger phrases cover all content types (patterns, standards, decisions).

### Over-Triggering

If the skill activates on queries where it should not:

- Add more specific negative conditions to the description.
- Ensure the "should NOT be used" clause explicitly excludes the triggering phrases.
- Narrow trigger phrases to focus on architectural and implementation intent.
