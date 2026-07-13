---
name: commit
description: >-
  Creates and executes one coherent git commit from staged or selected working-tree
  changes. Use when the user asks to commit changes, stage and commit a logical
  change, or create a git commit with an appropriate message. Do not use for a
  draft-only message request or an operation that only pushes existing commits.
allowed-tools: Bash(git add:*) Bash(git status:*) Bash(git diff:*) Bash(git commit:*) Bash(git log:*) Bash(git ls-files:*) Bash(git branch:*) AskUserQuestion
---

Create exactly one coherent commit while preserving changes outside its intended
scope. Write the commit message in English and communicate with the user in their
language. Follow explicit user instructions and applicable repository guidance
before the defaults below.

## Repository Context

- Status: !`git status --short`
- Staged changes: !`git diff --cached`
- Unstaged changes: !`git diff`
- Untracked files: !`git ls-files --others --exclude-standard`
- Unmerged files: !`git diff --name-only --diff-filter=U`
- Current branch: !`git branch --show-current`
- Recent commits: !`git log --oneline -10`

## 1. Define the Commit Scope

1. If the worktree and index contain no changes, tell the user there is nothing
   to commit and stop.
2. Select the target changes in this order:
   - If the user named files or a logical change, use that scope. If unrelated
     changes are already staged, explain the conflict and ask how to proceed
     before changing the index.
   - Otherwise, if staged changes exist, treat the staged diff as the complete
     target. Leave unstaged and untracked changes untouched.
   - Otherwise, group unstaged and untracked changes by purpose. If they form one
     clear logical change, use that group. If there are multiple plausible groups
     or a file's inclusion is ambiguous, present the groups and ask which one to
     commit.
3. Judge cohesion by behavior and purpose, not by file count alone. Keep tests,
   documentation, and configuration with the implementation they directly
   support.

## 2. Prepare the Index

When the exact target is not already staged, stage only its files with explicit
pathspecs:

```bash
git add -- "path/with[brackets]/file.ts" "another path/file.md"
```

Use `--` before paths and quote paths containing spaces, brackets, parentheses,
wildcards, or other shell-special characters. Include untracked files only when
they are part of the target change. Exclude local configuration, credentials,
temporary files, logs, and build artifacts unless repository guidance explicitly
tracks them and they are required by the change. If a target file mixes relevant
and unrelated hunks, stage only the relevant hunks; ask the user before staging
the whole file when safe partial staging is not practical.

After staging, re-run `git status --short` and `git diff --cached`. The staged diff
is now the sole source of truth for the commit.

## 3. Review the Staged Diff

Stop before committing when any blocking condition is present:

| Condition | Required action |
|-----------|-----------------|
| Empty staged diff | Tell the user there is nothing staged for this commit. |
| Unmerged entries or real conflict markers | List the affected files and ask the user to resolve them. |
| Suspected secret, credential, private key, or unintended `.env` file | Identify the location without repeating the value; ask the user to remove it or confirm it is non-secret test data. |
| Unclear or unrelated staged content | Explain the mismatch and ask how to scope the commit. |

Also inspect added lines for accidental debug statements, generated output, logs,
and unexplained `TODO` or `FIXME` markers. Ask only when an item appears unintended;
do not treat every occurrence as an error.

## 4. Compose the Message

First follow an explicit repository commit convention. Use recent commit history
as supporting evidence, not as a replacement for written guidance.

When no convention is defined, use these defaults:

- Start the subject with an imperative English verb such as `Add`, `Fix`,
  `Update`, `Remove`, or `Refactor`.
- Describe the primary change, omit a trailing period, and keep the subject at
  50 characters or fewer.
- Add a body only when the motivation, important tradeoff, breaking behavior, or
  migration detail is not clear from the subject and diff.
- Separate the body from the subject with a blank line, explain why the change
  was made, and wrap body lines at 72 characters.

Do not force a body based only on the number of changed files.

## 5. Verify and Commit

Before executing the commit, review `git diff --cached` one final time and verify:

1. Every staged change belongs to the selected scope.
2. The message describes the primary staged change without claiming unstaged work.
3. The staged diff still passes the safety review above.

For a subject-only message, run:

```bash
git commit -m "Subject line"
```

For a message with a body, use separate `-m` arguments so Git inserts a real blank
line. Put real line breaks in a long body; never encode them as literal `\n` text.

```bash
git commit \
  -m "Subject line" \
  -m "Explain why this change was necessary and include any important
migration detail."
```

If the commit fails, inspect the error and current status before taking further
action. If a hook modified files or the index, review the new staged diff and
revalidate the message before retrying. Do not retry a failure blindly.

After success, run `git log -1 --format='%h %s'` and `git status --short`. Report
the commit hash and subject, then mention any remaining unstaged or untracked
changes. Do not push unless the user separately requested it.
