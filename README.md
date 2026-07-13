# Your Repo Is the Prompt

[![skills.sh](https://skills.sh/b/0xd669/skills)](https://skills.sh/0xd669/skills)

Commit messages and PR descriptions aren't just for humans anymore. They become context for the coding agents that work in your repo next. That context compounds: good history sharpens the next agent's judgment, while sloppy history dulls it.

These skills bring engineering judgment to the moments when your agent creates durable context: commits, pull requests, and the prompts that guide its work.

## Install

```bash
npx skills add 0xd669/skills
bunx skills add 0xd669/skills
```

Then invoke `commit`, `pr`, or `prompt-doctor` using your agent's skill syntax.

## Why these skills?

**Your repo gives agents context before they write.** A coding agent doesn't have to start from a blank slate. It can draw from `git log`, `git blame`, the PR that introduced the code, and repository instructions such as `AGENTS.md` or `CLAUDE.md`. Together, they shape the agent's working context. A history of `fix bug` and `update files` hands it nothing. `Deduplicate retries by request ID` hands it the rule the system actually follows.

**What an agent writes becomes context for the next task.** Today's commit message can explain tomorrow's code. Humans may revisit repository history occasionally; agents can pull it into context with a single tool call. A clear message can keep paying off long after the original change ships.

## The skills

### `commit`: write history the next agent can use

Turns the right working-tree changes into one coherent commit while leaving unrelated work untouched.

- Writes messages that explain the behavior change, not the file list. The title names the changed rule; when needed, the body states the before-and-after transition.
- Groups changes by purpose and commits a single logical change. Unrelated WIP stays untouched.
- Stages only with explicit pathspecs, never with `git add -A`.
- Stops on suspected secrets, unintended `.env` files, and conflict markers; flags leftover debug statements before they enter the commit.

### `pr`: knowledge transfer, not a commit dump

Creates or updates the pull request for your branch, sized for today's reviewer and written for the humans or agents that revisit it later.

- Synthesizes final behavior from the complete diff against the PR's actual base. It keeps independent workflows distinct and reports rationale and validation only when supported by evidence.
- Scales the body to the diff: a two-commit fix gets a summary and a change list; a forty-file refactor gets themed sections. Never a wall of noise, never three lines for a rewrite.
- Updates in place without clobbering your edits: human-authored sections are preserved, and only generated content is regenerated.
- Extracts ticket IDs from branch names into the title (`TASK-123-fix-auth` → `[TASK-123] Fix auth`), and treats `wip/` and `draft/` branches as drafts.

### `prompt-doctor`: keep the explicit prompt sharp

Commits and PRs are the context your agent writes as a side effect. `AGENTS.md`, `CLAUDE.md`, system prompts, and agent instructions are the context you write on purpose, but they still rot. This skill diagnoses any LLM prompt against a defect taxonomy (contradictions, buried constraints, negative-heavy rules, untestable asks) and applies the smallest rewrite that fixes what is actually broken.

- Deletes before it adds. Most prompt "fixes" pile on more instructions; the usual cure is removing the noise that buries the rules you already have.
- Preserves your placeholders, template variables, and delimiters exactly.
- Works on system prompts, agent instructions, `AGENTS.md`, `CLAUDE.md`, `SKILL.md` bodies, and prompt strings embedded in code.

## Works with

Install through [skills.sh](https://skills.sh) in any supported coding agent.

---

Built by [0xd669](https://0xd669.com). Give your agent better judgment at the moments that matter.
