# ADR-005: Use pnpm Workspaces as the Monorepo Tool

## Status

Accepted

## Context

ConnectSpace consists of three primary packages:

- Client application
- Server application
- Shared TypeScript package

The repository requires local package linking and shared dependency management.

The primary options considered were:

- pnpm Workspaces
- Turborepo
- Nx

The project currently contains only a small number of packages and is developed by a single contributor.

## Decision

The repository will use pnpm Workspaces.

The workspace structure consists of:

- apps/
- packages/

Shared code will be imported through local workspace packages rather than duplicated between applications.

No additional orchestration layer will be introduced at this stage.

## Consequences

### Benefits

- Simple configuration.
- Excellent local package linking.
- Efficient dependency management.
- Minimal tooling overhead.
- Keeps attention focused on application architecture rather than build tooling.

### Trade-offs

- No build caching.
- No advanced task orchestration.
- Manual coordination of development scripts.
- Fewer large-scale monorepo features.

These trade-offs are acceptable because the repository currently contains only a small number of packages.

## Future Reconsideration

This decision should be revisited if:

- Additional applications are introduced.
- Multiple shared packages emerge.
- Build times become significant.
- Repository complexity increases substantially.

If those conditions arise, adopting Turborepo would provide caching, task orchestration, and dependency-aware builds while preserving the existing workspace structure.