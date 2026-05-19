# Conventional Commit for Codex

A Codex skill for generating Conventional Commit messages with required scope and for handling reliable `commit and sync` workflows.

This skill aligns with [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) and adds a stricter house rule:
- always require a scope
- allow the standard common extended types
- allow the custom `seo` type

## Format

This skill enforces:

```txt
<type>(<scope>): <description>
```

Examples:
- `fix(auth): correct session refresh behavior`
- `feat(api): add export endpoint`
- `seo(sitemap): exclude utility pages from indexing`
- `refactor(ui): simplify confirmation state`

## What It Helps With

- generating repo-consistent Conventional Commit messages
- choosing a useful `type`
- requiring a meaningful `scope`
- checking git state before commit
- completing `commit and sync` safely
- verifying the branch is no longer ahead after push

## Supported Types

- `feat`
- `fix`
- `refactor`
- `chore`
- `build`
- `ci`
- `docs`
- `test`
- `perf`
- `style`
- `seo`
- `revert`

## Suggested Scopes

Examples:
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

## Install

Codex skills are folder-based and require a `SKILL.md` file.

Recommended structure:

```txt
conventional-commit/
  SKILL.md
```

Install by copying the folder into:

```txt
~/.codex/skills/conventional-commit/
```

Then restart Codex so the skill is discovered.

## Use

Trigger it with prompts like:
- `commit msg`
- `commit and sync`
- `write a commit message for this diff`
- `commit these changes`
- `use conventional commit naming`

## Notes

- This skill is stricter than the base Conventional Commits spec because it requires a scope for every commit.
- `seo` is a custom extension type and not part of the core Conventional Commits keywords.
- For `commit and sync`, the task is not complete until the branch is no longer shown as `ahead` in `git status -sb`.
