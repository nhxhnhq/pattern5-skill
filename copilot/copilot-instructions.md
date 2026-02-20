# Pattern5: Organizational Governance

Before implementing features, choosing approaches, setting up projects, or making architectural decisions, query the Pattern5 MCP server for organizational standards and patterns.

## Workflow

1. **Search first.** Call `pattern5_search` with the technology and concern (e.g., `"server action error handling"`). Call `pattern5_get` on the best match to retrieve full guidance.
2. **Apply by type.** Standards are rules — follow them. Patterns are guidance — adapt them. Decisions are prior choices — respect them. Principles are trade-off guidelines — evaluate their algorithmic logic.
3. **Report gaps.** When no artifact exists, create a draft with `pattern5_submit` specifying the appropriate `type` (`pattern`, `standard`, `decision`, or `principle`) and all required sections.

## Artifact Types

**Standards** define what MUST, SHOULD, or MAY be done. Read the `rule`, `scope`, `compliant_examples`, and `non_compliant_examples` sections. Check enforcement level: `must` (mandatory), `should` (expected), `may` (optional).

**Patterns** provide reusable solutions. Read `apply_when` to confirm fit, `do_not_apply_when` to rule out false matches, `structure` for implementation shape, `key_constraints` for hard rules, and `anti_patterns` for what to avoid.

**Decisions** record architectural choices. Check `decision_status`: `active` (follow it), `superseded` (find replacement), `deprecated` (proceed with caution). Read `context`, `rationale`, and `consequences`.

**Principles** are hierarchical trade-off guidelines (parent → sub-principles, max 3 levels) with priority 1-10 (10 = highest). Read the `algorithmic_expression` to evaluate IF/THEN/ELSE/UNLESS conditions against the current task. When principles conflict, higher priority wins; when tied, prefer the more specific sub-principle. Check `conflict_notes` for resolution guidance.

## Available Tools

| Tool | Purpose |
|------|---------|
| `pattern5_search` | Search artifacts by keyword |
| `pattern5_list` | Browse artifacts, filter by type or status (use `status='draft'` for drafts) |
| `pattern5_get` | Retrieve full artifact details |
| `pattern5_rate` | Rate an artifact after applying it |
| `pattern5_submit` | Create a draft artifact (specify `type`: pattern, standard, decision, or principle) |
| `pattern5_update` | Modify an existing artifact |
| `pattern5_manage` | Publish, unpublish, or dismiss an artifact (owner only) |

## When NOT to Query

Do not search Pattern5 for syntax questions, debugging runtime errors, third-party library docs, or general programming knowledge. The trigger is architectural intent — choosing *how* to build something, not *what* a keyword means.

## Versioning

Version 1.2.0. The MCP server is authoritative for tool schemas. If the server differs from these instructions, trust the server.
