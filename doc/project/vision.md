# Product vision

> **Reviewed requirements baseline:** Accepted by the maintainer on 2026-11-01 under [REQ-01 (#2)](https://github.com/aaliakbarian/postgis_versioning/issues/2) as the foundation for Phase 1 design. This document states intent and boundaries; detailed, traceable requirements are in [requirements.md](requirements.md).

## Purpose

`postgis_versioning` is intended to be a PostgreSQL extension for versioning PostGIS `geometry` data and its relational attributes through a SQL-first interface. It should let database users work with mutable editing workspaces and retained historical states without making an application service part of the extension.

The central product boundary is one registered logical table at a time. A table should support both collaborative editing and retained updates from external sources, while logical versions remain distinct from physical checkpoints and storage maintenance.

## Intended outcomes

- Editors can isolate geometry and attribute changes, inspect history and differences, reconcile conflicts, and post changes to a shared target.
- Data operators can retain frequent full deliveries, infrequent replacements, and supplied change batches as queryable historical states.
- Users can create immutable snapshots and explicitly fork them into mutable workspaces.
- Maintainers can evolve storage, checkpoints, compaction, and retention without changing the meaning of retained logical states.
- PostgreSQL owns database connection authentication and database privileges. External applications own application authentication, business approvals, and monetization policy. The extension protects its integrity within PostgreSQL's security model.

These are intended capabilities, not implemented features or a commitment that all will ship in the first release.

## Accepted product boundaries

- Versioning is independent for each registered logical table containing PostGIS `geometry` columns and associated relational attributes.
- The initial scope has no extension-level multi-table version groups, cross-table workflow coordination, or high-level dataset/catalog abstraction.
- SQL is the primary public interface.
- Mutable workspaces, branching, immutable snapshots, explicit snapshot forks, history, comparisons, reconciliation, conflict handling, posting, bulk ingestion, and lifecycle maintenance are in the long-term product scope.
- Logical version semantics are separate from physical storage representation and checkpoint policy.
- ArcGIS and Git may inform terminology or comparisons, but neither is a compatibility target.

## Agreed preferences

- Prefer container-based development and testing, with minimal WSL host dependencies.
- Allow temporarily incomplete spatial work inside isolated workspaces, then enforce configured spatial rules at a defined integration boundary. The precise boundary, supported rules, and feasibility constraints remain unresolved.
- Neither preference authorizes bypassing PostgreSQL privileges or disabling required database constraints.

## Exclusions and deferrals

- Application authentication and approval workflows, REST services, raster versioning, object-storage management, map serving, and initial Citus-specific implementation are excluded.
- `geography` is deferred beyond the initial scope.
- Native PostGIS `TopoGeometry` integration is unresolved.
- Raw drive-test observations are outside the initial product use cases; this scope statement does not require the extension to infer their business meaning or reject an otherwise supported table because it contains observations.
- Processed telecom or radio coverage outputs may be suitable retained states, but they are not necessarily PostGIS polygonal coverages and must not be confused with polygon coverage validation.
- License selection remains pending.

## Initial release boundary

No first-release feature set is selected by this baseline. The maintainer should select it only after the dependent work defines:

- version, state, workspace, reconciliation, and posting semantics;
- supported table shapes, feature identity, and the public SQL contract;
- storage correctness, delta completion, retention, and recovery guarantees;
- spatial validation boundaries; and
- history, attribution, and counting contracts.

The implementation language, PostgreSQL/PostGIS compatibility baseline, SQL/native split, physical layout, and performance targets also remain open. The candidate release must be a coherent, testable subset of the accepted scope, but choosing that subset requires approval.
