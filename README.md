# Small Correct Diff

**A Codex plugin and portable Agent Skill for keeping AI-written code small without turning it into code golf.**

Coding agents sometimes add more than a task needs: new helpers, layers,
dependencies, speculative configuration, and broad refactors. Small Correct
Diff gives the agent a stricter optimization order:

> **Correctness → Safety → Architecture → Simplicity → Diff size → LOC**

The goal is not to write the shortest code. The goal is to write the smallest
correct and complete change.

## What it does

Before adding code, the agent checks, in order:

1. Does this need to exist at all?
2. Does the repository already have something reusable?
3. Can the standard library do it?
4. Can the native platform do it?
5. Can an already-installed dependency do it?
6. Can it be implemented directly without another abstraction?
7. Only then: add the minimum new code.

For bug fixes, it also asks the agent to inspect callers and fix the root cause
rather than patch only the reported symptom.

## What it does not optimize away

Small Correct Diff explicitly puts these above brevity:

- correctness and completeness
- security and authorization
- data integrity
- validation at trust boundaries
- error handling that prevents data loss or corruption
- required concurrency and transactional guarantees
- accessibility
- explicitly requested behavior

A tiny wrong patch is worse than a slightly larger correct one.

## Install in Codex

Add this repository as a plugin marketplace, then install the plugin:

```bash
codex plugin marketplace add alkalinejapan/small-correct-diff
codex plugin add small-correct-diff@small-correct-diff
```

Restart Codex and start a new task after installation.

## Other Agent Skills-compatible clients

Copy `skills/small-correct-diff` into the skills location used by your agent.
Keep the directory name `small-correct-diff` so it matches the `name` in
`SKILL.md`.

To apply the policy continuously to one Codex project, copy `AGENTS.md` to that
repository's root. If the repository already has an `AGENTS.md`, merge the
rules instead of replacing its project-specific instructions.

## Structure

```text
small-correct-diff/
├── .agents/plugins/marketplace.json
├── .codex-plugin/plugin.json
├── skills/small-correct-diff/SKILL.md
├── AGENTS.md
├── README.md
└── LICENSE
```

The manifest and `skills/` layout follow the current plugin format shared by
ChatGPT and Codex. `AGENTS.md` remains separate because it is an optional,
always-on project policy.

## When to use it

Good fits include feature implementation, bug fixes, refactoring, dependency
decisions, code review, and keeping agent-generated pull requests focused.

It is less useful when the explicit goal is exploration, architecture
brainstorming, throwaway prototyping, or generating several alternative
designs.

## Token usage

Smaller diffs can reduce generated code and sometimes reduce token usage, cost,
or latency. Those effects depend on the model and task. This project has not
yet published its own benchmark, so it does not claim a specific savings rate.

## Inspiration

This project was inspired by [Ponytail](https://github.com/DietrichGebert/ponytail),
especially its emphasis on YAGNI, reuse, standard-library and native solutions,
and resisting over-engineering.

Small Correct Diff is an independent implementation with a different priority:
correctness, safety, and architectural consistency come before minimizing diff
size or lines of code. It is not affiliated with or endorsed by Ponytail.

## License

MIT. See [LICENSE](LICENSE).
