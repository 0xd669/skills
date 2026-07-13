# Repository Guidance

## Purpose

This repository is a personal library of reusable agent skills for recurring work and life tasks. Prefer a small set of skills proven useful in real use over a large or sophisticated catalog.

A skill earns its place when it repeatedly saves time, reduces errors or decision fatigue, or produces a meaningfully better result than an unassisted agent.

## Repository Boundaries

- `skills/<name>/SKILL.md` is the source for each skill. Keep optional `scripts/`, `references/`, `assets/`, and `agents/openai.yaml` beside it only when they serve the skill directly.
- `README.md` is the user-facing catalog and installation guide.
- `.claude-plugin/` contains Claude Code distribution metadata. Change it only when packaging, identity, installation, or release metadata changes; keep it synchronized with the README.
- `experiments/` contains local evaluation artifacts and is not committed.
- Keep this file limited to durable, repository-wide guidance. Put task-specific workflows in skills instead of duplicating them here.

## Working Agreements

- Inspect the current files and git state before editing. Preserve unrelated user changes.
- Proceed autonomously when the requested outcome is clear. Ask only when a missing decision would materially change the result.
- Do not commit, push, publish, or open a pull request unless the user explicitly asks.
- Communicate in the user's language; default to Korean for Korean requests. Write commit messages and pull request titles in English and pull request bodies in Korean.
- Write reusable skill instructions in clear English unless the task itself depends on another language. Do not force a fixed output language unless it is part of the skill's contract.

## Decide Before Creating a Skill

1. Search existing skills first. Extend an existing skill when the new behavior serves the same job and trigger boundary.
2. Create a skill only for a reusable workflow, non-obvious domain knowledge, or resources that improve repeated execution. Use an ordinary prompt for one-off behavior and this file only for recurring repository-wide conventions.
3. Start from concrete requests that should trigger the skill, boundary or non-trigger requests, expected results, and important failure modes. Derive these from available context when possible; ask the user only for material gaps.
4. Before authoring, define what the skill helps the user accomplish or decide, representative inputs, observable success criteria, allowed evidence or resources, and important constraints or side effects.

## Authoring Skills

- Give each skill one focused job. Use a short lowercase hyphenated directory name, and make it exactly match the frontmatter `name`.
- Every `SKILL.md` requires valid YAML frontmatter with `name` and `description`. Add other fields only when a target runtime actually needs them.
- Make `description` the routing contract: front-load the job and strongest trigger terms, and state important non-triggers when overlap with another skill or ordinary behavior is plausible.
- Keep the body concise, imperative, and limited to information a capable agent would not reliably infer. Add role labels, fixed scaffolds, examples, and warnings only when they fix an observed failure or make non-obvious behavior testable.
- Match specificity to risk: use flexible guidance when several approaches are valid, a preferred procedure when consistency matters, and deterministic scripts when mistakes are costly or steps are repeatedly reimplemented.
- Use `scripts/` for deterministic or repeated operations, `references/` for knowledge loaded only when needed, `assets/` for files copied into outputs, and `agents/openai.yaml` only for UI metadata, invocation policy, or tool dependencies.
- State inputs, outputs, evidence boundaries, side effects, and failure behavior when they materially improve execution. Require confirmation before irreversible or externally visible actions unless the user already authorized them.
- Avoid runtime-specific syntax and tool names unless the skill targets that runtime. When runtime specificity is necessary, say so explicitly and keep the rest portable.
- Do not add auxiliary documentation inside a skill. Put user-facing installation and catalog information in the root README.

## Validation

Before considering skill work complete:

1. Run the validator provided by the active target runtime or skill creator. If none is available, manually verify valid YAML, directory and `name` agreement, a clear `description`, and all referenced files; report the missing automated validation without treating it as a blocker.
2. Run every changed script on a representative input and check its actual output and failure behavior.
3. For a meaningful behavior change, build a small evaluation set from the defined examples and success criteria. Include trigger, boundary or non-trigger, and failure or edge cases.
4. Run the previous version or an unassisted baseline and the candidate on the same evaluation set in fresh sessions when practical. Judge both against the same criteria and record unsupported or ambiguous results; do not claim improvement from prompt inspection alone.
5. Review the final diff for broken references, stale metadata, accidental generated files, and unrelated changes. Update `README.md` when skills are added, removed, renamed, or their invocation or installation changes.

## Learning From Use

- Treat user corrections and failed outputs as evidence. Update a skill when the lesson is reusable, and add or revise a validation case that would have exposed the failure.
- Do not encode one-off preferences or incidents as permanent rules.
- Merge or remove skills that overlap or no longer demonstrate enough recurring value to justify maintenance.
- Update this file only for recurring repository-wide mistakes or conventions. Keep narrower guidance with the skill it governs.
