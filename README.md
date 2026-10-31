# postgis_versioning

A PostgreSQL extension project for per-table versioning of PostGIS geometry data and its relational attributes.

The project aims to support collaborative editing and retained historical states through a PostgreSQL-native SQL interface.

## Project status

**Early design and research.**

The repository currently contains the project scaffold and initial planning issues. An installable extension is not yet available.

The implementation language, storage architecture, public SQL interface, and PostgreSQL/PostGIS compatibility baseline remain under investigation.

## Planned capabilities

- Independent versioning of registered tables.
- Isolated editing workspaces and parent/child branching.
- Immutable historical snapshots with explicit forks into mutable workspaces.
- Feature history, state comparisons, and audit-relevant metadata.
- Reconciliation, conflict detection, conflict resolution, and posting.
- Dedicated bulk snapshot and change-batch ingestion operations.
- Physical checkpoints, asynchronous maintenance, compaction, and configurable retention.
- Spatial validation using existing PostGIS capabilities.

These are intended capabilities, not implemented features.

## Primary use cases

### Collaborative editing

Users edit geometry and attributes in isolated workspaces. Applications coordinate review and determine when changes should be posted to a shared target version.

### Historical source updates

Periodic full snapshots or supplied changes produce retained historical states that external systems can query and compare.

A registered table should support both workflows.

## Scope and boundaries

The initial spatial scope is PostGIS `geometry` columns and associated relational attributes. Versioning operates independently for each registered logical table.

SQL is the primary interface. PostgreSQL authentication and database privileges remain PostgreSQL responsibilities.

Application authentication, business approval workflows, REST services, raster versioning, object-storage management, map serving, and initial Citus-specific integration are outside this repository.

`geography` support is deferred. Native PostGIS `TopoGeometry` integration requires a separate design decision.

## Repository layout

| Directory | Purpose |
| --- | --- |
| `doc/project/` | Product context, requirements, and roadmap |
| `doc/architecture/` | Architecture, decision records, and research |
| `doc/development/` | Development and testing instructions |
| `src/` | Future implementation source |
| `sql/` | Future extension SQL |
| `docker/` | Container development and testing configuration |
| `scripts/` | Repository tooling |
| `tests/` | Regression, concurrency, and fixture resources |
| `benchmarks/` | Reproducible performance measurements |
| `experiments/` | Bounded research prototypes |
| `.github/workflows/` | Future CI workflows |

The directories are currently scaffold placeholders.

## Development approach

The development workflow uses WSL2 Ubuntu 24.04, VS Code, GitHub CLI, Docker, and Docker Compose.

Build and integration-testing dependencies will run in containers where practical, keeping host installation requirements small.

Development proceeds through reviewed requirements, architecture decisions, representative experiments, and small testable milestones.

## Planning

- [Issues](https://github.com/aaliakbarian/postgis_versioning/issues)
- [Milestones](https://github.com/aaliakbarian/postgis_versioning/milestones)

## License

License selection is pending.
