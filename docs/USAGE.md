# Usage Guide

## Quick Start

Once installed, use the skill by typing `/test` followed by what you want to test.

## Mode 1: "Test This" (Targeted)

Use this when you want to test a specific piece of code.

### Examples

```
/test this — test the function I just wrote
/test src/auth/middleware.ts
/test the checkout flow
/test this PR's changes
```

### What Happens

1. Analyzes the target code and its dependencies
2. Identifies edge cases and error paths
3. Writes tests matching your project's existing style
4. Runs the tests
5. Reports: tests added, passing/failing, coverage change

### Best For

- After writing new code
- After fixing a bug (regression test)
- Before submitting a PR
- When you're unsure if something works correctly

## Mode 2: "Test Everything" (Comprehensive Audit)

Use this when you want to improve overall project test health.

### Examples

```
/test everything
/test audit the whole project
/test find coverage gaps
```

### What Happens

1. Scans all source files and existing tests
2. Identifies files with no tests or weak coverage
3. Prioritizes by risk:
   - **Critical**: Auth, payments, API endpoints, data mutations
   - **High**: Shared utilities, state management
   - **Medium**: UI logic, config parsing, CLI handlers
   - **Low**: Pure rendering, generated code, type-only files
4. Generates tests in batches (asks before continuing)
5. Delivers a summary report

### Best For

- New projects that need a test foundation
- Projects with low coverage that need improvement
- Before major refactors (safety net)
- Periodic test health checks

## Tips

### Let it use your patterns

The skill reads your existing tests first. If you have specific conventions (test file naming, mock setup, fixture patterns), it will follow them.

### Trust the prioritization

Not all code needs tests equally. The skill focuses on code that could break your users — not vanity coverage.

### Review the bug reports

When the skill finds actual bugs during testing (not test bugs), it reports them clearly. These are real issues worth fixing.

### Iterate

For large projects, "test everything" works in batches. Review each batch before proceeding — you might want to adjust priorities based on what you see.
