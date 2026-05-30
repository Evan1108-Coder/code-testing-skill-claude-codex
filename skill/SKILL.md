---
description: "Use this skill when testing is needed — writing tests, running test suites, analyzing coverage gaps, or validating changes. Two modes: 'test this' for targeted testing of specific changes, and 'test everything' for comprehensive project-wide test audits. Adapts to any test framework (Jest, Vitest, pytest, Go test, etc.)."
---

# Intelligent Test Generation & Execution

Two modes of operation based on user intent:

## Mode 1: "Test This" (Targeted)

Triggered when the user wants to test specific changes, a file, a function, or a feature.

### Workflow

1. **Analyze scope** — Identify what changed or what needs testing:
   - Read the target files/functions
   - Map dependencies (what calls this? what does this call?)
   - Identify the public interface vs internal implementation
   - Note edge cases from the code logic (null paths, error branches, boundary values)

2. **Detect test infrastructure** — Before writing anything:
   - Find existing test files (look for `*.test.*`, `*.spec.*`, `*_test.*`, `test_*.*`, `tests/` dirs)
   - Identify the framework: check package.json (`jest`, `vitest`, `mocha`), pyproject.toml (`pytest`), go.mod, Cargo.toml, etc.
   - Check for test config files (`jest.config.*`, `vitest.config.*`, `pytest.ini`, `.pytest.ini`, `conftest.py`)
   - Note existing patterns: how are mocks set up? fixtures? test utilities?
   - Check if there's a test runner script in package.json or Makefile

3. **Generate test plan** — Before writing code, outline:
   - Which functions/methods need tests
   - What scenarios to cover (happy path, error cases, edge cases)
   - What to mock vs what to test with real implementations
   - Expected assertions

4. **Write tests** — Following the project's existing patterns:
   - Match the style of existing tests (naming, structure, assertions)
   - Use the same test utilities and helpers already in the project
   - Group related tests logically (describe/context blocks)
   - Each test should be independent and deterministic
   - Prefer testing behavior over implementation details

5. **Run and iterate** — Execute the tests:
   - Run only the new/relevant tests first (not the full suite)
   - If tests fail due to test bugs, fix the tests
   - If tests fail due to actual code bugs, report them clearly
   - Run the full suite to check for regressions
   - Report: X tests added, Y passing, Z coverage delta (if coverage tool available)

## Mode 2: "Test Everything" (Comprehensive Audit)

Triggered when the user wants to improve overall test coverage or audit testing health.

### Workflow

1. **Scan the project** — Build a testing inventory:
   - List all source files and their corresponding test files (or note which lack tests)
   - Check current coverage metrics if available (`npx vitest --coverage`, `pytest --cov`, etc.)
   - Identify the test-to-source ratio
   - Note any test infrastructure issues (broken tests, skipped tests, flaky tests)

2. **Prioritize by risk** — Not all code needs equal coverage:
   - **Critical** (test first): Public API endpoints, auth/security, payment/billing, data mutations, core business logic
   - **High**: Shared utilities used by many modules, state management, data transformations
   - **Medium**: UI components with logic, configuration parsing, CLI command handlers
   - **Low** (test last): Pure UI rendering, generated code, simple getters/setters, type-only files

3. **Coverage gap analysis** — Identify what's missing:
   - Functions with no tests at all
   - Functions with only happy-path tests (missing error/edge cases)
   - Integration points with no integration tests
   - Code paths that only execute under specific conditions

4. **Generate tests in batches** — Work systematically:
   - Start with critical-priority items
   - Write tests for one module/file at a time
   - Run after each batch to ensure they pass
   - Report progress: "Batch 1: auth module — 12 tests added, all passing"
   - Ask before proceeding to the next batch if the scope is large

5. **Summary report** — When complete:
   - Total tests added
   - Coverage before/after (if measurable)
   - Remaining gaps and their priority
   - Any bugs discovered during testing
   - Recommendations for test infrastructure improvements

## Test Quality Principles

- **Test behavior, not implementation** — Assert on outputs and side effects, not internal state
- **One assertion concept per test** — Each test should verify one logical thing (may use multiple `expect` calls for one concept)
- **Descriptive test names** — `it("returns 404 when user not found")` not `it("test error")`
- **Arrange-Act-Assert** — Clear separation of setup, execution, and verification
- **No test interdependence** — Each test must pass in isolation and in any order
- **Minimal mocking** — Only mock external services, time, and randomness. Don't mock the thing you're testing.
- **Deterministic** — No flaky tests. Avoid real network calls, real timers, real file system state that isn't cleaned up.

## Framework Detection Cheat Sheet

| Signal | Framework | Run Command |
|--------|-----------|-------------|
| `vitest` in package.json | Vitest | `npx vitest run` |
| `jest` in package.json | Jest | `npx jest` |
| `@testing-library/*` | RTL (with Jest/Vitest) | Same as above |
| `mocha` in package.json | Mocha | `npx mocha` |
| `pytest` in requirements/pyproject | pytest | `pytest` |
| `unittest` imports | Python unittest | `python -m pytest` (usually works) |
| `_test.go` files | Go testing | `go test ./...` |
| `#[cfg(test)]` in .rs | Rust | `cargo test` |
| `*.test.ts` + `deno.json` | Deno | `deno test` |
| `*.spec.rb` | RSpec | `bundle exec rspec` |
| `ExUnit` in mix.exs | ExUnit | `mix test` |

## Anti-Patterns to Avoid

- Don't test private/internal functions directly (test through the public API)
- Don't write tests that just duplicate the implementation logic
- Don't mock everything — integration tests with real dependencies catch real bugs
- Don't generate 100 trivial tests for 100% coverage — focus on meaningful assertions
- Don't skip running the tests after writing them
- Don't add tests that are already covered by existing tests
- Don't test framework/library behavior (trust that `Array.map` works)
