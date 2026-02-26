# spec-kit-featcomp

A [Spec Kit](https://github.com/github/spec-kit) extension for tracking feature completion with status headers and a central registry.

## What It Does

After you merge a feature branch to `main`, this extension:

1. **Validates** all tasks in the feature's `tasks.md` are complete
2. **Adds status headers** to `spec.md`, `plan.md`, and `tasks.md`
3. **Updates** a central `specs/STATUS.md` registry
4. **Commits** the tracking changes

### Status Header Format

Each spec document gets a blockquote header after its first heading:

```markdown
# Feature Specification: DB Schema Explorer

> **STATUS: COMPLETE** | Merged: 2026-02-02 | Branch: `001-db-schema-explorer`
```

### Central Registry

`specs/STATUS.md` maintains a table of all features:

| Feature ID | Name | Branch | Status | Merged Date | Notes |
|------------|------|--------|--------|-------------|-------|
| 001 | DB Schema Explorer | `001-db-schema-explorer` | Complete | 2026-02-02 | All phases implemented |

## Installation

```bash
# From GitHub release
specify extension add --from https://github.com/jesse-smith/spec-kit-featcomp/archive/refs/tags/v1.0.0.zip

# Or from local clone (for development)
git clone https://github.com/jesse-smith/spec-kit-featcomp
specify extension add --dev spec-kit-featcomp/
```

### Migrating from Dev to Release

If you previously installed with `--dev`, remove the dev version first and then install from the release:

```bash
specify extension remove featcomp
specify extension add --from https://github.com/jesse-smith/spec-kit-featcomp/archive/refs/tags/v1.0.0.zip
```

This does not affect your project's `specs/` directory or any existing `STATUS.md` data — only the extension files under `.specify/extensions/featcomp/` are replaced.

## Usage

After merging a feature branch:

```bash
git checkout main
git merge 001-db-schema-explorer
```

Then in your AI agent:

```
/speckit.featcomp.complete
```

Or with an explicit feature name:

```
/speckit.featcomp.complete 001-db-schema-explorer
```

The alias `/speckit.complete` also works.

## Prerequisites

- [Spec Kit](https://github.com/github/spec-kit) installed (`>=0.1.0`)
- Feature branches follow the `###-feature-name` convention
- Features have spec documents in `specs/###-feature-name/`

## What Gets Validated

Before marking a feature complete, the extension checks:

- **Branch**: Must be on `main`, not a feature branch
- **Tasks**: All tasks in `tasks.md` must be checked off (`- [x]`)

If `specs/STATUS.md` doesn't exist yet, it's created from the included template.

## Optional: Merge Reminder Hook

If you use [hookify](https://github.com/anthropics/claude-code), you can add a reminder that triggers on feature branch merges.

Create `.claude/hookify/feature-merge-reminder.md`:

```markdown
---
name: feature-merge-reminder
enabled: true
event: bash
pattern: git\s+merge\s+\d{3}-
action: warn
---

**Feature branch merge detected!**

Run `/speckit.featcomp.complete` to:
- Add status headers to spec.md, plan.md, tasks.md
- Update specs/STATUS.md registry
```

## Suggested CLAUDE.md Snippet

Add this to your project's `CLAUDE.md` for agent awareness:

```markdown
## Feature Completion Workflow

When a feature branch is merged to main:

1. Run `/speckit.featcomp.complete` to update status tracking:
   - Adds completion headers to spec.md, plan.md, tasks.md
   - Updates the central `specs/STATUS.md` registry
2. Commit the status updates

See `specs/STATUS.md` for the current feature registry.
```

## License

MIT
