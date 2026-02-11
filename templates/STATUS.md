# Feature Status Registry

Central registry of all features and their current status.

| Feature ID | Name | Branch | Status | Merged Date | Notes |
|------------|------|--------|--------|-------------|-------|

## Status Definitions

- **Draft**: Initial specification, not yet approved
- **In Progress**: Active development on feature branch
- **Complete**: All tasks finished, merged to main
- **Archived**: Feature closed without full implementation (see Notes)

## Usage

When completing a feature branch merge:

1. Run `/speckit.featcomp.complete` to update this registry
2. Status headers are added to spec.md, plan.md, tasks.md
3. Commit the status updates
