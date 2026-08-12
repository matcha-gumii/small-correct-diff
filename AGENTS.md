# Small Correct Diff

For coding changes, optimize for the **smallest correct and complete change**.

Correctness and completeness outrank brevity. Do not omit required behavior
merely to reduce code, files, or diff size.

## Priority

When goals conflict:

1. Correctness and completeness
2. Security and data integrity
3. Existing architecture consistency
4. Simplicity
5. Small diff / few changed files
6. Few lines of code

## Before writing code

Understand the task and trace the relevant code path first. Inspect related
implementations and callers before choosing where to edit.

Then use this order:

1. Does this need to be built at all? Follow YAGNI.
2. Does the codebase already contain a helper, utility, type, component,
   service, or pattern that solves it? Reuse it.
3. Can the standard library solve it? Use it.
4. Can a native platform feature solve it? Prefer it.
5. Can an already-installed dependency solve it cleanly? Reuse it.
6. Can it be implemented directly and clearly without a new abstraction?
   Do that.
7. Only then write the minimum new code required.

## Implementation rules

- Prefer deletion over addition when behavior stays correct.
- Prefer boring, obvious code over clever code.
- Minimize changed files and diff size only after locating the correct layer.
- Do not add abstractions unless they solve a concrete current need.
- Do not add interfaces with one implementation, factories for one product,
  configuration for values that never vary, or scaffolding for hypothetical
  future requirements.
- Do not add dependencies when the standard library, platform, existing
  dependencies, or a few clear lines are sufficient.
- Do not duplicate functionality already present in the repository.
- Do not perform unrelated cleanup unless needed for the requested change.

## Bug fixes

Fix the root cause, not just the reported symptom.

Before changing shared code, inspect its callers and surrounding flow.
Prefer one correct fix at the shared boundary over repeated patches at
individual call sites when appropriate.

A smaller diff in the wrong place is not a better fix.

## Never simplify away

- validation at trust boundaries
- authentication / authorization
- security controls
- error handling needed to prevent data loss or corruption
- required transactional or concurrency guarantees
- accessibility requirements
- explicitly requested behavior

When two approaches are similarly simple, choose the edge-case-correct one.

## Tests

For non-trivial new logic, add the smallest useful runnable check or test that
would fail if the behavior broke. Prefer the repository's existing test
infrastructure.

Do not create elaborate test infrastructure unless needed.

Run the most relevant available checks before finishing.

## Before finishing

Review the diff:

- Did I write anything unnecessary?
- Did I duplicate something already available?
- Did I introduce an abstraction before it was needed?
- Did I add a dependency unnecessarily?
- Did I touch unrelated code?
- Can any new code or file be deleted while preserving correctness?
- Did I fix the root cause?
- Did I preserve security, validation, data integrity, error handling,
  accessibility, and all requested behavior?

If so, simplify before finishing.
