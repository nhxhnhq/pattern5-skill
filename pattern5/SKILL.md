---
name: pattern5
description: This skill should be used when the user is creating a new feature, choosing an implementation approach, setting up a project, making a technology decision, adding a new component or module, onboarding to a codebase, or discussing architectural trade-offs. It should NOT be used for syntax questions, debugging runtime errors, general programming knowledge, or tasks unrelated to the connected codebase.
license: MIT
compatibility: Requires a Pattern5 MCP server connection. The MCP server must be configured in the agent's MCP settings with a valid API key or OAuth token. Without a server connection, this skill's tools will not be available.
metadata:
  author: NHXHN
  version: 1.1.0
  mcp-server: pattern5
  category: governance
  tags: [standards, patterns, decisions, principles, architecture, mcp]
  docs: https://pattern5.com
  support: support@pattern5.com
---

# Pattern5: Organizational Governance for AI Agents

Query the organization's Pattern5 library before making architectural decisions. The library contains four artifact types that serve different purposes:

- **Standards** are prescriptive rules. They define what MUST, SHOULD, or MAY be done. Compliance is expected.
- **Patterns** are reusable solutions. They provide structure, constraints, and anti-patterns for recurring problems. Adoption is recommended.
- **Decisions** are architectural decision records. They capture why a choice was made, what alternatives were considered, and the consequences. They inform future choices.
- **Principles** are hierarchical decision-making guidelines. They encode trade-off logic as algorithmic IF/THEN expressions that agents can evaluate. They sit above other artifact types as the most abstract form of governance.

Treat Pattern5 as the organization's source of truth for how code should be written, structured, and evolved. The workflow is: search for existing guidance, apply what is found (differentiating by type), and report gaps when no guidance exists.

## Step 1: Search Before Building

Before writing implementation code for a feature, component, or architectural change, search Pattern5 for existing guidance. This is the most important step -- it prevents violations of organizational standards and avoids duplicating solved problems.

### When Starting a New Task

Call `pattern5_search` with a query that names the technology and concern (e.g., `"server action error handling"`, `"database migration workflow"`). For effective query strategies, consult `references/query-strategies.md`.

When `pattern5_search` returns results, scan the titles and descriptions to identify the most relevant match. Call `pattern5_get` on that artifact to retrieve the full content before writing code. Search results only contain titles and summaries -- the full artifact includes implementation details, code examples, constraints, and verification checklists.

If search returns multiple artifacts that look relevant, retrieve the top two or three. They often serve complementary roles -- one may be a standard (what must be done) while another is a pattern (how to do it) or a decision (why it was done this way).

If search returns no results, try alternative terms or synonyms before concluding there is no guidance. Then try `pattern5_recommend` with the project's technology stack for broader discovery. A genuine gap should be reported (see Step 3).

### When Onboarding to a Project

Call `pattern5_sync_project` with the technologies detected in the project's dependency files (package.json, requirements.txt, go.mod, pyproject.toml, Cargo.toml, etc.) and the project type (`web`, `api`, `mobile`, `cli`, `desktop`, or `library`). This automatically finds artifacts matching the tech stack and assigns them to the project.

After syncing, call `pattern5_list` to see what artifacts are now available. This provides a quick overview of the organization's standards and conventions for the project's technology stack.

### When Exploring What Exists

Call `pattern5_list` to browse all available artifacts. Filter by type (`pattern`, `standard`, `decision`, or `principle`) to narrow results. This is useful when beginning work in an unfamiliar area of the codebase.

Call `pattern5_recommend` with the project's technology stack to get ranked recommendations with relevance scores and match reasons. This is more targeted than listing all artifacts -- it surfaces the most relevant ones based on the technologies in use.

## Step 2: Apply What Is Found

Differentiate how artifacts are applied based on their type.

### Applying Standards

Standards are the highest-priority artifacts. They represent organizational rules that code must comply with.

Each standard has an enforcement level that determines compliance expectations:

- **must** -- Mandatory. Violation requires explicit justification and approval. Flag any deviation to the user.
- **should** -- Expected. Deviation is acceptable with documented reasoning. Note the deviation and explain why.
- **may** -- Optional. Follow when it adds value to the current context.

When a standard applies to the current task, read these sections in order:

1. **rule** -- The prescriptive requirement. Understand what is mandated or prohibited.
2. **scope** -- What the standard governs. Confirm the current task falls within scope.
3. **compliant_examples** -- Code that follows the rule. Model implementation after these examples.
4. **non_compliant_examples** -- Code that violates the rule. Ensure the implementation avoids these patterns.
5. **exceptions** (if present) -- Approved cases where the rule can be broken. Check whether the current context qualifies before deviating.

If the task involves code that falls within a standard's scope, write code that matches the compliant examples. When compliance is not possible, inform the user and explain the conflict.

### Applying Patterns

Patterns provide recommended approaches, not mandates. They describe reusable solutions to recurring problems.

Read the pattern's sections in this order:

1. **apply_when** -- Conditions that indicate this pattern fits. Confirm the current situation matches.
2. **do_not_apply_when** -- Conditions that rule out the pattern. If any match, do not apply it.
3. **structure** -- The implementation shape. Follow this as the architectural blueprint.
4. **key_constraints** -- Hard rules within the pattern. These must be respected when applying the pattern.
5. **anti_patterns** -- Common mistakes. Actively avoid these during implementation.
6. **verification** -- A checklist of assertions. Walk through each item after implementation to confirm correct application.

Patterns are guidance, not law. Adapt the structure to fit the specific context. If the pattern does not quite fit, note the adaptation and reasoning.

### Applying Decisions

Decisions record architectural choices already made. They are not instructions to follow but context to respect.

Check the `decision_status` field before acting on a decision:

- **active** -- The decision is current. Follow it. Do not re-litigate settled choices without new information.
- **superseded** -- A newer decision replaced this one. Find and follow the replacement.
- **deprecated** -- The decision is no longer recommended. Proceed with caution and consider proposing a new decision.

Read the decision's sections to understand the full context:

1. **context** -- The circumstances that drove the decision. Understand the constraints that were in play.
2. **decision_outcome** -- What was chosen. This is the choice to follow (if active).
3. **rationale** -- Why this choice was made. This explains the reasoning and trade-offs.
4. **alternatives_considered** (if present) -- Options that were evaluated and rejected. Avoid re-proposing these unless circumstances have materially changed.
5. **consequences** -- Positive and negative impacts. Anticipate these trade-offs in the current work.

If proposing a different approach than an active decision, reference the existing decision explicitly and explain what has changed to warrant revisiting it.

### Applying Principles

Principles encode trade-off logic that guides decisions across the organization. They are hierarchical: a parent principle can have sub-principles that refine it, up to three levels deep.

#### Priority and Conflict Resolution

Each principle has an optional priority from 1 (lowest) to 10 (highest). When two principles apply to the same situation:

1. **Higher priority wins.** A priority-8 principle overrides a priority-5 principle on the same topic.
2. **Specificity breaks ties.** Prefer a sub-principle over its general parent when both match the current context.
3. **Check conflict notes.** Many principles include a `conflict_notes` section that explicitly addresses overlap with other principles. Read this before applying competing principles.

#### Reading a Principle

Read the principle's sections in this order:

1. **rationale** -- Why this principle exists. Understand the reasoning and trade-offs that motivated it.
2. **algorithmic_expression** -- The actionable logic. This uses IF/THEN/ELSE/UNLESS keywords to express when and how the principle applies. Evaluate the conditions against the current task.
3. **examples** -- Concrete illustrations of the principle in action. These show both correct and incorrect applications.
4. **conflict_notes** (if present) -- Guidance on what to do when this principle conflicts with another.

#### When to Query for Principles

Search for principles when:

- **Competing concerns arise** -- e.g., performance vs. maintainability, speed vs. safety, flexibility vs. consistency.
- **No standard or pattern exists** -- Principles provide higher-level guidance when specific rules have not been written yet.
- **Technology trade-offs need resolution** -- e.g., choosing between approaches where both have valid merits.
- **A decision requires justification** -- Principles supply the reasoning framework that decisions can reference.

#### Example Workflow

1. The agent searches for guidance on infrastructure provisioning and receives a principle titled "Prefer Managed Services Over Self-Hosted."
2. The principle's `algorithmic_expression` contains:
   ```
   IF deploying a new service
     IF a managed equivalent exists with acceptable cost
       THEN use the managed service
       UNLESS regulatory constraints require on-premises hosting
     ELSE
       DOCUMENT why self-hosting is necessary
   ```
3. The agent evaluates the conditions: a managed equivalent exists, cost is within budget, no regulatory constraints apply.
4. The agent proceeds with the managed service and cites the principle in the decision rationale.

### Rating Artifacts

After applying an artifact, call `pattern5_rate` with a rating from 1 to 5 and an optional comment. This feedback improves artifact quality and recommendation accuracy.

## Step 3: Report Gaps

When no relevant artifact exists for a decision or approach being taken, create a draft to capture the knowledge for future reference.

### Creating Draft Artifacts

Choose the appropriate submission tool based on what is being documented:

- **Reusable solution to a recurring problem** -- Call `pattern5_submit_pattern` with a title, description, and `apply_when` conditions. Include `key_constraints`, `anti_patterns`, and `structure` when the implementation shape is clear.
- **Prescriptive rule or guideline** -- Call `pattern5_submit_standard` with a title, `rule`, and `scope`. Include `compliant_examples` and `non_compliant_examples` with concrete code snippets when possible.
- **Architectural choice with rationale** -- Call `pattern5_submit_decision` with a title, `decision` (what was chosen), and `context` (what drove the choice). Include `rationale`, `alternatives_considered`, and `consequences` to make the decision record complete.
- **Trade-off guideline or guiding belief** -- Call `pattern5_submit_principle` with a `title`, `description`, `rationale`, `algorithmic_expression`, and `examples`. The `algorithmic_expression` should use IF/THEN/ELSE/UNLESS keywords with one rule per line. Optionally include `conflict_notes`, `technologies`, `layer`, `parent_id` (to create a sub-principle), `priority` (1-10, where 10 is highest), and `status`.

All submissions are saved as drafts by default. Provide technology tags to improve discoverability. Set the `layer` field (`presentation`, `application`, `data`, or `infrastructure`) when the artifact targets a specific architectural layer.

Write draft content as if another developer -- or another AI agent -- will read it months from now. Clear titles, specific descriptions, and concrete examples make artifacts useful beyond the current session.

### Managing Drafts

Call `pattern5_drafts` to list artifacts awaiting review. Use `pattern5_update` to modify a draft's title, description, sections, technologies, or layer. Call `pattern5_publish` to make a draft available to all agents in the project. Call `pattern5_unpublish` to revert a published artifact back to draft status. Call `pattern5_delete` to dismiss an artifact that is no longer needed.

## When NOT to Query Pattern5

Not every task requires organizational governance. Do not search Pattern5 for:

- **Syntax or language questions** -- General programming knowledge does not belong in an organizational library. Questions like "How does async/await work?" or "What does the spread operator do?" are answered by training data.
- **Debugging runtime errors** -- Error messages, stack traces, and breakpoints are task-specific. Fixing a TypeError on line 42 does not require a governance check.
- **Third-party library documentation** -- Use the library's own docs, a documentation tool, or the library's README. Pattern5 stores organizational decisions about *which* libraries to use, not how to use them.
- **General knowledge** -- Questions answerable from training data do not need organizational context. Algorithm explanations, language comparisons, and conceptual overviews fall outside Pattern5's scope.
- **Unrelated tasks** -- If the current task does not involve architecture, implementation approach, or coding standards, skip the query. File renaming, typo fixes, and simple formatting changes do not need governance review.

The trigger is architectural intent: choosing *how* to build something, not *what* a language keyword means. When in doubt, ask: "Am I making a design choice that future developers will need to understand or follow?" If yes, search Pattern5 first.

## Version Compatibility

This skill is version 1.1.0. The MCP server is the authoritative source for tool schemas, parameter names, and response formats. If the server's tool interface differs from these instructions, trust the server. Tool schemas are self-describing and always reflect the current API.

## Additional Resources

### Reference Files

For guidance on writing effective queries and interpreting results, consult:
- **`references/query-strategies.md`** -- Content type selection, search query examples, and result interpretation
