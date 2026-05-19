---
name: conventional-commit
description: "Use this skill whenever the user asks to commit, push, sync, or generate a Conventional Commit message. It enforces required scope and handles reliable commit-and-sync workflows."
---

# Conventional Commit

A Codex skill for generating Conventional Commit messages in the format
`<type>(<scope>): <description>` and for completing `commit and sync` safely.

This skill aligns with Conventional Commits 1.0.0:
https://www.conventionalcommits.org/en/v1.0.0/

It applies a stricter house rule on top of the spec:
- always require a scope
- allow the standard common extended types
- allow the custom `seo` type

## When To Use

Use this skill when:
- the user asks for a commit message
- the user says `commit msg`
- the user says `commit and sync`
- the user says `commit`
- the user says `sync`
- the user says `push`
- the user asks to commit changes
- the user asks to push changes
- the user asks to sync the branch
- the user wants a pending diff summarized into a commit subject
- the user wants commit wording that matches the repo's style

This skill is required whenever the user asks to commit, sync, or push.
Do not perform a commit or push directly from general repo habits when one of
those triggers is present. Use this skill first, even if you already know how
to run the git commands yourself.

## What This Skill Does

This skill helps Codex:
- inspect the current git state
- read the current diff
- choose an appropriate Conventional Commit type and scope
- generate a commit subject in the required format
- if asked to `commit and sync`, create the commit, push it, and verify the branch is no longer ahead

## Required Format

Always use:

`<type>(<scope>): <description>`

Examples:
- `fix(auth): correct session refresh behavior`
- `feat(api): add export endpoint`
- `seo(sitemap): exclude utility pages from indexing`
- `refactor(ui): simplify confirmation state`

Rules:
- `type` is required
- `scope` is required
- `description` is required
- keep the description short, specific, and grounded in the actual diff
- use lowercase for the description unless a proper noun requires capitalization
- do not end the subject with punctuation
- prefer a subsystem, feature, or area name for the scope rather than a filename
- choose the narrowest meaningful scope that still reads clearly

## Allowed Types

Prefer this set:
- `feat` for new user-facing or developer-facing functionality
- `fix` for bug fixes, regressions, or corrections
- `refactor` for structural code changes without intended behavior change
- `chore` for maintenance, cleanup, or non-product housekeeping
- `build` for build tooling, dependencies, bundling, packaging, or deployment config
- `ci` for CI/CD workflows and automation
- `docs` for documentation-only changes
- `test` for tests and test-only support changes
- `perf` for performance improvements
- `style` for formatting-only or presentational cleanups with no logic change
- `seo` for sitemap, robots, canonical, metadata, structured data, indexing, and crawl-control changes
- `revert` for reversions

Note:
- `seo` is a custom extension, not a core Conventional Commits keyword, but it is allowed because Conventional Commits permits additional types

## Scope Guidance

Prefer scopes like:
- `auth`
- `api`
- `ui`
- `routing`
- `forms`
- `search`
- `status`
- `sitemap`
- `seo`
- `config`
- `deps`
- `deps-dev`
- `deploy`

Avoid:
- omitting scope
- overly broad scopes when a clearer subsystem exists
- file-by-file scopes unless the file maps directly to a stable subsystem

## Workflow

1. Check pending work with `git status --short`.
2. Check branch sync state with `git status -sb`.
3. Inspect the diff with:
   - `git diff --stat`
   - `git diff --cached --stat`
   - targeted `git diff` reads for the main touched files
4. Sample recent history with `git log --oneline -n 20`.
5. Determine whether the pending work is one coherent change or multiple unrelated changes.
6. Choose the best `type` from the allowed set.
7. Choose a meaningful required `scope`.
8. Write one best commit subject in the required format.
9. If the user asks only for a commit message, return the best subject.
10. If the user asks to `commit and sync` or otherwise asks you to commit:
   - stage the intended files
   - create the commit with the chosen subject
   - push the current branch to its upstream
   - re-check `git status -sb`
   - if the user asked to `sync` or `push`, treat that as part of this same required workflow
11. Consider sync incomplete if the branch still shows `ahead` after push. Keep going until it is no longer ahead, or clearly report the blocker.

## Decision Rules

- prefer `fix` over `refactor` when the primary value is correcting behavior, layout, content, or UI
- prefer `refactor` when behavior should stay the same and the change is structural
- prefer `build` for dependency bumps and toolchain changes
- prefer `seo` for sitemap, robots, canonical, indexing, and metadata work
- if the diff mixes unrelated work, say so before suggesting a message
- do not invent prefixes, scopes, or behaviors not supported by the actual diff

## Good Examples

- `fix(ui): stabilize dropdown interactions`
- `feat(api): add background job endpoint`
- `build(deps-dev): upgrade framework tooling`
- `seo(sitemap): exclude utility pages from indexing`

## Avoid

- `fix: update stuff`
- `updated settings page`
- `Refine responsive layout`
- `chore(misc): changes`

## Output

When the user is asking only for a message:
- start with the best commit message in backticks
- add a short summary only when helpful
- mention mixed scope if the diff is not coherent

When the user asks to `commit and sync`:
- use the workflow above instead of stopping at a suggestion
- report:
  - the commit message used
  - the commit hash
  - whether push succeeded
  - whether `git status -sb` confirms the branch is no longer ahead

When the user asks to `commit`, `push`, or `sync`:
- do not bypass this skill
- use the same workflow and reporting style that applies to `commit and sync`
