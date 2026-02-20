# Query Strategies for Pattern5

## Content Type Selection

Choose the right artifact type based on the situation:

| Situation | Artifact Type | Why |
|-----------|--------------|-----|
| Implementing a feature or solving a recurring problem | **Pattern** | Patterns provide reusable solutions with structure, constraints, and anti-patterns |
| Checking if something is allowed or required | **Standard** | Standards define prescriptive rules with enforcement levels (must/should/may) |
| Understanding why a past choice was made | **Decision** | Decisions capture rationale, alternatives considered, and consequences |
| Resolving architectural trade-offs or competing concerns | **Principle** | Principles encode trade-off logic as IF/THEN expressions with priority-based conflict resolution |
| Setting up a new project or onboarding | **All four** | Use `pattern5_list` to discover all relevant artifacts for the project |
| Proposing a new technology or approach | **Decision** | Check existing decisions first to avoid revisiting settled choices |

## Effective Search Queries

### Good Queries

Specific, intent-driven queries produce the best results:

- `"server action error handling"` - Names the feature and concern
- `"database migration workflow"` - Targets a specific process
- `"React component composition"` - Combines technology and pattern
- `"API authentication"` - Clear domain and concern
- `"rate limiting implementation"` - Specific capability
- `"performance vs maintainability tradeoff"` - Surfaces guiding principles
- `"infrastructure provisioning guidelines"` - Broad principle-level guidance

### Poor Queries

Avoid vague or overly broad terms:

- `"best practices"` - Too generic, matches everything
- `"code"` - Not specific enough
- `"how to"` - Phrased as a question rather than a topic
- `"fix"` - Describes an action, not a subject area

### Tips

- Use 2-4 words that name the technology and concern together.
- If searching returns too many results, add a technology qualifier (e.g., `"Next.js caching"` instead of `"caching"`).
- If searching returns no results, broaden the query by removing qualifiers or try synonyms.

## Interpreting Results

### Multiple Results

When `pattern5_search` returns several artifacts:

1. Scan titles and descriptions to identify the most relevant match.
2. Use `pattern5_get` to retrieve the full content of the best candidate.
3. If two artifacts seem equally relevant, retrieve both -- one may be a pattern (how) while the other is a standard (rule).

### No Results

When no artifacts match:

1. Try alternative terms or synonyms (e.g., `"auth"` vs `"authentication"`).
2. Use `pattern5_list` with a type filter to browse available artifacts in that category.
3. If the gap is genuine, create a draft artifact to capture the missing guidance.

### When to Use pattern5_get

Always call `pattern5_get` before applying an artifact. Search results provide titles and summaries, but the full artifact contains:

- Detailed implementation guidance and code structure
- Constraints, anti-patterns, and verification checklists
- Enforcement levels (for standards) and decision status (for decisions)
- Algorithmic logic (for principles)
- Technology tags and layer classification
