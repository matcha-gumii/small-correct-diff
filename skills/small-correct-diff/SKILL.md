---
name: small-correct-diff
description: Keep coding-agent changes correct, complete, safe, and small. Use when implementing features, fixing bugs, refactoring, reviewing code, or changing dependencies. Prefer existing code, standard libraries, native platform features, and direct implementations over speculative abstractions, boilerplate, new dependencies, or broad rewrites.
---

# Small Correct Diff

Optimize for the **smallest correct and complete change**, not the fewest lines at any cost.

## Priority order

When goals conflict, use this order:

1. Correctness and completeness
2. Security and data integrity
3. Consistency with the existing architecture and codebase
4. Simplicity
5. Small diff and few changed files
6. Few lines of code

Never sacrifice a higher-priority item to improve a lower-priority one.

## Before writing code

First understand the task and trace the relevant code path end to end.
Inspect related implementations and callers before deciding where to edit.

Then stop at the first option that fully solves the problem:

1. **Does this need to exist at all?** Follow YAGNI.
2. **Does the codebase already solve it?** Reuse an existing helper, utility,
   type, component, service, or pattern.
3. **Can the standard library solve it?** Use it.
4. **Can a native platform feature solve it?** Prefer it.
5. **Can an already-installed dependency solve it cleanly?** Reuse it.
6. **Can it be implemented directly and clearly without a new abstraction?**
   Do that.
7. **Only then add new code.** Add the minimum required.

Run this ladder after understanding the problem, never instead of understanding it.

## Implementation rules

- Prefer deletion over addition when behavior remains correct.
- Prefer boring, obvious code over clever code.
- Do not introduce speculative abstractions or scaffolding.
- Do not create an interface with one implementation, a factory for one
  product, or configuration for values that do not vary unless there is a
  concrete current need.
- Do not add a dependency when existing code, the standard library, the
  platform, an installed dependency, or a few clear lines are sufficient.
- Do not duplicate functionality already present in the repository.
- Minimize files and diff size only after choosing the correct implementation
  location.
- Do not omit required behavior merely to reduce code, files, or diff size.
- Do not perform unrelated cleanup unless it is necessary for the requested
  change.

## Bug fixes

Fix the root cause, not only the reported symptom.

Before editing shared code, inspect its callers and surrounding flow.
Prefer one correct fix at a shared boundary over repeated guards at individual
call sites when the shared fix is semantically correct.

A smaller diff in the wrong layer is not a better fix.

## Never simplify away

Do not weaken or remove:

- input validation at trust boundaries
- authentication or authorization checks
- security controls
- error handling needed to prevent data loss or corruption
- transactional or concurrency guarantees required for correctness
- accessibility requirements
- explicitly requested behavior

When two approaches are similarly simple, choose the one that handles edge
cases correctly.

## Tests and verification

For non-trivial logic, leave the smallest useful runnable check that would fail
if the behavior breaks. Prefer the repository's existing test infrastructure.

Do not create new test frameworks, fixture systems, or broad suites unless the
task requires them.

Run the most relevant available checks before finishing.

## Before finishing

Review the diff and ask:

- Did I add anything unnecessary?
- Did I duplicate something already available?
- Did I introduce an abstraction before it was needed?
- Did I add a dependency unnecessarily?
- Did I change unrelated code?
- Can code or files be deleted without reducing correctness?
- Did I fix the root cause?
- Did I preserve validation, security, data integrity, error handling,
  accessibility, and requested behavior?

Simplify only when all higher-priority requirements remain satisfied.

## Output behavior

Keep explanations concise unless the user requests a detailed walkthrough.
Mention meaningful simplifications or deliberately skipped speculative work
when useful.
