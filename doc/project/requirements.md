# Product requirements

> **Reviewed requirements baseline:** Accepted by the maintainer on 2026-11-01 under [REQ-01 (#2)](https://github.com/aaliakbarian/postgis_versioning/issues/2) as the foundation for Phase 1 design. Nothing here claims an implemented feature or selects an implementation architecture.

## 1. How to read this document

Requirement identifiers are stable references for review and acceptance scenarios. Status terms follow the [documentation index](../README.md):

- **Accepted boundary (`BND-*`)** records supplied scope.
- **Derived requirement (`REQ-*`)** is an accepted product requirement in this baseline; detailed semantics and initial-release inclusion remain governed by the recorded open work.
- **Agreed preference (`PREF-*`)** records an expressed direction while leaving its precise contract or feasibility open.
- **Goal or recommendation (`REC-*`)** retains stable identifiers from the earlier draft; each row states whether it is an expressed goal or still requires approval.
- **Open decision (`OPEN-*`)** must be resolved by the linked design or research work.

Maintainer acceptance applies to the `BND-*` boundaries, `REQ-*` requirements, and `AC-*` acceptance scenarios. `REC-*` goals or recommendations, `PREF-*` preferences with unresolved mechanisms, and `OPEN-*` decisions retain the individual status recorded below.

The cited PostgreSQL and PostGIS versions are evidence versions, not a selected compatibility baseline.

## 2. Accepted boundaries

| ID | Boundary |
| --- | --- |
| BND-001 | `postgis_versioning` is a PostgreSQL extension whose primary public interface is SQL. |
| BND-002 | Each registered logical table is versioned independently, including its PostGIS `geometry` columns and associated relational attributes. |
| BND-003 | A registered table supports both collaborative editing and retained historical source updates. |
| BND-004 | The initial scope has no extension-level multi-table version groups or cross-table workflow coordination. |
| BND-005 | Mutable workspaces, branching, immutable historical snapshots, and explicit forks from snapshots into mutable workspaces are product concepts. Their precise state model remains open. |
| BND-006 | Logical versions and states are distinct from physical storage, deltas, and checkpoints. |
| BND-007 | PostgreSQL owns database connection authentication and database privileges. External applications own application authentication, business approvals, and monetization policy. The extension protects its integrity within PostgreSQL's security model. |
| BND-008 | Application approval states and roles such as “manager” are not extension-level version states or required extension roles. |
| BND-009 | REST services, raster versioning, object-storage management, map serving, and initial Citus-specific implementation are excluded. |
| BND-010 | `geography` is deferred; native `TopoGeometry` integration remains unresolved. |
| BND-011 | A high-level application dataset/catalog abstraction is outside the initial scope. |
| BND-012 | ArcGIS and Git may inform terminology and comparison, but neither defines a compatibility contract. |
| BND-013 | Raw drive-test observations are outside the initial product use cases, but the extension is not required to infer their business meaning or reject an otherwise supported table because it contains observations. Processed telecom or radio coverage outputs may be registered versioned data; they are not necessarily PostGIS polygonal coverages. |
| BND-014 | License selection remains pending. |

## 3. Derived requirements

### 3.1 Registration and logical isolation

| ID | Requirement |
| --- | --- |
| REQ-REG-001 | A user can register a supported logical table for versioning without grouping it with other tables. |
| REQ-REG-002 | Versioning preserves the table's geometry and relational attributes as one logical feature record, subject to later decisions about feature identity and supported table shapes. |
| REQ-REG-003 | Operations on one registered table do not require cross-table workflow coordination by the extension. |

### 3.2 Editing, snapshots, and comparison

| ID | Requirement |
| --- | --- |
| REQ-EDIT-001 | Users can edit geometry and relational attributes in isolated mutable workspaces. |
| REQ-EDIT-002 | Users can branch work, inspect workspace changes and history, reconcile conflicts, and post accepted changes to an eligible shared target. |
| REQ-EDIT-003 | Extension behavior does not require an application manager role or business-approval state. |
| REQ-SNAP-001 | Users can retain immutable historical snapshots and query their logical contents. |
| REQ-SNAP-002 | A user can explicitly fork an immutable snapshot into a new mutable workspace; the snapshot itself remains immutable. |
| REQ-CMP-001 | Users can compare relevant logical states. The comparison interface and output contract remain open. |

### 3.3 Ingestion and historical access

| ID | Requirement |
| --- | --- |
| REQ-ING-001 | A full delivery can produce a retained logical state while keeping earlier retained states accessible. |
| REQ-ING-002 | Frequent full deliveries must have a design path for practical ingestion and storage efficiency, but this baseline sets no performance target or checkpoint cadence. |
| REQ-ING-003 | An infrequent full replacement can become a retained state without requiring synchronous computation of a full-dataset diff before the replacement is accepted. |
| REQ-ING-004 | A supplied change batch is applied atomically: it becomes one accepted logical change or leaves no partial accepted state. |
| REQ-ING-005 | Historical states before and after an accepted batch remain accessible subject to the approved retention contract. |
| REQ-ING-006 | Batch validity, feature identity, ordering, retry, duplicate, and replay behavior must be defined before the change-batch interface is accepted. |
| REQ-COV-001 | Processed telecom or radio coverage outputs can use the same retained-state model when represented by a supported registered table. Suitability for versioning does not imply that the geometries form a PostGIS polygonal coverage. |
| REQ-COV-002 | Raw drive-test observations are outside the initial product use cases. This scope boundary does not require the extension to infer business meaning or reject an otherwise supported table because it contains observations. |

PostgreSQL transactions provide an all-or-nothing basis for atomic database changes, but the public operation boundary and recovery contract still belong to later SQL and storage design. A PostgreSQL `COMMIT` ends a database transaction; it does not by itself define an extension-recorded state or accepted logical transition ([PostgreSQL 18 transactions](https://www.postgresql.org/docs/18/tutorial-transactions.html), [PostgreSQL 18 `COMMIT`](https://www.postgresql.org/docs/18/sql-commit.html)).

### 3.4 History, attribution, and counts

| ID | Requirement |
| --- | --- |
| REQ-HIST-001 | Users can inspect useful logical feature history for retained accepted transitions and other retained work as defined by the approved history contract. |
| REQ-HIST-002 | The system can report attributable change counts, while the meanings of “change,” “feature,” “actor,” import counts, reverted edits, and posted changes remain unresolved. |
| REQ-HIST-003 | An unavailable or not-yet-derived history or count result must not be represented as a known zero or a completed empty result. |
| REQ-HIST-004 | The history contract distinguishes database identity from application-supplied attribution. Trusted attribution mechanisms remain open. |

### 3.5 Maintenance and retention

| ID | Requirement |
| --- | --- |
| REQ-MNT-001 | Physical maintenance, checkpointing, compaction, and garbage collection must preserve reconstructability and logical contents of retained states, together with the lineage, provenance, bases, and dependencies required by the approved versioning, reconciliation, history, and retention contracts. This does not require every ancestor or physical delta or checkpoint to survive indefinitely; exact lineage retention and pruning semantics remain open. |
| REQ-MNT-002 | Asynchronous maintenance exposes no partially activated logical state; activation, retries, crash recovery, and availability before derived work completes require design. |
| REQ-RET-001 | Retention is configurable per registered table. |
| REQ-RET-002 | Retention must protect active-workspace bases and other dependencies required by retained snapshots, active workspaces, running jobs, and the approved history/audit contract. The exact protected references and exceptions remain open. |

Time-based, change-volume-based, and adaptive checkpoints are candidates for storage design, not accepted algorithms or initial-release promises.

### 3.6 Validation, privileges, and integrity

| ID | Requirement |
| --- | --- |
| REQ-VAL-001 | Validation requirements distinguish individual geometry validity, polygon coverage rules, and dataset-specific network connectivity. |
| REQ-VAL-002 | Validation of an integration result considers the resulting contents, including relevant unchanged neighboring features, when the configured rule requires them. |
| REQ-SEC-001 | PostgreSQL owns connection authentication and database privileges. Public extension operations enforce the applicable PostgreSQL privileges and do not bypass database security. |
| REQ-SEC-002 | The extension protects its integrity within PostgreSQL's security model. The design must not depend on disabling required database constraints; a rejected operation leaves no partially accepted logical change. |

`ST_IsValid` tests whether an individual geometry is well-formed in 2D. Polygon coverage checks have separate conditions such as non-overlap and edge matching, and network connectivity requires its own domain contract; one check cannot be promised to cover all three. These PostGIS polygonal coverage concepts are distinct from the business meaning of processed telecom or radio coverage outputs ([PostGIS 3.6 `ST_IsValid`](https://postgis.net/docs/manual-3.6/ST_IsValid.html), [PostGIS 3.6 `ST_CoverageInvalidEdges`](https://postgis.net/docs/manual-3.6/ST_CoverageInvalidEdges.html)). PostgreSQL documents connection authentication, object privileges, and constraints as database security and integrity mechanisms that the extension must work with, not bypass ([PostgreSQL 18 client authentication](https://www.postgresql.org/docs/18/client-authentication.html), [PostgreSQL 18 privileges](https://www.postgresql.org/docs/18/ddl-priv.html), [PostgreSQL 18 constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)).

## 4. Acceptance scenarios

### AC-EDIT-001: collaborative editing and posting

- **References:** BND-002, BND-007, BND-008; REQ-REG-001, REQ-REG-002, REQ-REG-003, REQ-EDIT-001, REQ-EDIT-002, REQ-EDIT-003, REQ-SNAP-002, REQ-CMP-001, REQ-HIST-001, REQ-HIST-002, REQ-HIST-003, REQ-HIST-004, REQ-SEC-001, REQ-SEC-002.
- **Input:** A registered table, an eligible shared target, and isolated workspaces containing geometry and attribute edits from one or more users.
- **User-visible outcome:** Each workspace isolates its edits. Authorized users can inspect changes, feature history, available attributable counts, and state comparisons; fork a snapshot into a workspace; detect and reconcile conflicts; and post an accepted result without requiring an extension-level manager approval state. Unavailable counts are distinguishable from zero, and attribution distinguishes database identity from application-supplied identity.
- **Examples:**
  - If the target and workspaces Alpha and Beta show feature A with `status = open`, an Alpha edit to `status = closed` is visible in Alpha while the target and Beta retain `status = open`.
  - If Alpha was forked from snapshot S, editing Alpha does not change S.
  - If Alpha and Beta share a base where feature A has `status = open`, and they change it to `closed` and `deferred` respectively, integration handles the divergence without silently losing either proposed value. Conflict representation and resolution remain open.
  - A history or count result still awaiting derivation is distinguishable from a completed result containing no changes.
- **Failure expectation:** A rejected reconciliation or post does not partially change the accepted target. PostgreSQL privilege or integrity failures are surfaced rather than bypassed.
- **Unresolved assumptions:** Feature identity, edit-recording boundaries, conflict granularity, stale-target behavior, repeated posting, validation boundary, and exact SQL interfaces.

### AC-FULL-001: frequent full deliveries

- **References:** BND-003, BND-006; REQ-ING-001, REQ-ING-002, REQ-SNAP-001, REQ-MNT-001, REQ-MNT-002, REQ-RET-001, REQ-RET-002.
- **Input:** Large recurring full deliveries for one registered table, such as daily parcel data.
- **User-visible outcome:** Each accepted delivery identifies a retained logical state; earlier retained states remain queryable, and physical optimization does not change their meaning.
- **Example:** If activation of today's parcel delivery fails, yesterday's accepted parcel state remains queryable and today's partial delivery is not presented as accepted.
- **Failure expectation:** A failed ingestion does not expose a partial delivery as an accepted state. The future operation contract must define failure reporting and retry behavior.
- **Unresolved assumptions:** Row identity across deliveries, checkpoint policy, delta timing, operations allowed before derived deltas complete, ingestion limits, and retention duration.

### AC-REPL-001: infrequent full replacements

- **References:** BND-003, BND-006; REQ-ING-001, REQ-ING-003, REQ-SNAP-001, REQ-HIST-003, REQ-MNT-002, REQ-RET-002.
- **Input:** A large authoritative replacement delivered at a long interval, such as an annual update.
- **User-visible outcome:** The accepted replacement becomes a retained logical state and the prior retained state remains accessible, without requiring a synchronous full-dataset diff as an acceptance prerequisite.
- **Example:** An annual replacement may become the accepted retained state before its derived feature-level diff is complete; until derivation finishes, the diff or related count is reported as unavailable or not ready rather than as a completed empty result.
- **Failure expectation:** Activation is atomic from the user's perspective; an incomplete replacement does not become the accepted state.
- **Unresolved assumptions:** Whether and when a derived diff is produced, what history exists before it is ready, retry and recovery behavior, and applicable retention protection.

### AC-BATCH-001: supplied change batch

- **References:** REQ-ING-004, REQ-ING-005, REQ-ING-006, REQ-HIST-001, REQ-SEC-001, REQ-SEC-002.
- **Input:** An external batch describing geometry and attribute changes for one registered table, together with the identifiers and ordering metadata required by the future batch contract.
- **User-visible outcome:** A valid batch is applied atomically and produces a retained historical result that authorized users can inspect.
- **Example:** If a batch contains valid changes for features A and B followed by an invalid item for feature C, none of the batch is exposed as an accepted result.
- **Failure expectation:** Invalid, unauthorized, conflicting, duplicate, replayed, or out-of-order input does not produce a partial accepted state. Exact retry and idempotency behavior must be defined before implementation.
- **Unresolved assumptions:** Batch identity, feature identity, ordering, validity rules, duplicate detection, retry tokens, target preconditions, and attribution.

### AC-COV-001: processed coverage outputs

- **References:** BND-002, BND-013; REQ-COV-001, REQ-COV-002, REQ-ING-001, REQ-VAL-001, REQ-VAL-002.
- **Input:** A processed telecom or radio coverage result represented as a supported table of geometry and attributes. Raw drive-test observations are outside this initial use case.
- **User-visible outcome:** Each accepted processed result may be retained and queried as a logical state using the same per-table versioning boundary.
- **Example:** A processed radio-signal result may contain points, lines, or polygons and still be considered for per-table versioning; it is not assumed to be a PostGIS polygonal coverage. An otherwise supported table is not rejected merely because its rows represent observations that an application chooses to register.
- **Failure expectation:** A failed import does not expose a partial accepted coverage state. Configured validation failures are reported at the future integration boundary.
- **Unresolved assumptions:** Coverage feature identity, source provenance, validation rules, integration boundary, and whether the processing pipeline supplies full replacements or change batches.

## 5. Goals, preferences, and recommendations

| ID | Status | Direction | Consequence or unresolved guarantee |
| --- | --- | --- | --- |
| REC-HIST-001 | Expressed goal; guarantees unresolved | Aim for complete logical change history for retained accepted transitions across collaborative editing, full source deliveries, replacements, and supplied change batches. This goal does not require recording every intermediate workspace edit or SQL statement, and PostgreSQL `COMMIT` does not define the extension's recorded-state boundary. | SEM-01, STO-01, and AUD-01 must define recording boundaries, feature identity, granularity, asynchronous delta completion, readiness reporting, and retention effects. Bulk replacements may be accepted before a derived diff is complete, and unavailable results must not appear as completed empty results. |
| PREF-VAL-001 | Agreed preference | Allow temporarily incomplete spatial work inside isolated workspaces and enforce configured spatial rules at a defined integration boundary. | VAL-01 must define the precise boundary, supported rules, relevant neighboring features, failure behavior, and feasibility constraints. This preference does not authorize bypassing PostgreSQL privileges or disabling required constraints. |
| REC-REL-001 | Recommendation requiring approval | Select the initial release only after issues #4–#8 establish a coherent semantics, SQL, storage, validation, and history slice. | This baseline does not preselect a feature list or imply that every long-term capability belongs in the first release. |
| PREF-DEV-001 | Agreed preference | Keep compilation and database/testing dependencies in containers where practical, with minimal WSL host installation. | Environment work must verify images, versions, architecture, and reproducible commands before documenting them as supported. |

Identifier mapping from the earlier draft:

- `REC-VAL-001` is now `PREF-VAL-001` because the direction is already an expressed preference.
- `REC-DEV-001` is now `PREF-DEV-001` for the same reason.
- `REC-HIST-001` and `REC-REL-001` retain their identifiers; no unrelated identifier changed.

## 6. Open decisions and ownership

| ID | Decision | Follow-up owner |
| --- | --- | --- |
| OPEN-SEM-001 | Version/state semantics, extension-recorded-state and accepted-transition boundaries distinct from PostgreSQL transaction boundaries, workspace and snapshot behavior, reconciliation, conflict granularity, posting, and historical-query intent. | [SEM-01 #4](https://github.com/aaliakbarian/postgis_versioning/issues/4) |
| OPEN-STO-001 | Authoritative records, physical layout, checkpoints, compaction, asynchronous delta completion, readiness reporting, crash recovery, exact lineage retention and pruning, retention exceptions, and dependency protection. | [STO-01 #5](https://github.com/aaliakbarian/postgis_versioning/issues/5) |
| OPEN-SQL-001 | Feature identity, supported table shapes, registration, editing and query interfaces, SQL signatures, operation boundaries, concurrency, and privileges. | [SQL-01 #6](https://github.com/aaliakbarian/postgis_versioning/issues/6) |
| OPEN-VAL-001 | Geometry, coverage, and network validation mechanisms; integration boundary; native `TopoGeometry` relationship. | [VAL-01 #7](https://github.com/aaliakbarian/postgis_versioning/issues/7) |
| OPEN-AUD-001 | Complete logical-history guarantees for retained accepted transitions, feature identity and granularity, intermediate workspace-edit treatment, actor attribution, count definitions, derived-result readiness, audit evidence, and retention effects. | [AUD-01 #8](https://github.com/aaliakbarian/postgis_versioning/issues/8) |
| OPEN-TECH-001 | C versus Rust/pgrx, SQL/native split, maintained toolchains, and supported PostgreSQL/PostGIS versions. | [TECH-01 #9](https://github.com/aaliakbarian/postgis_versioning/issues/9) |
| OPEN-REL-001 | The exact first-release feature set and explicit release acceptance criteria. | Maintainer approval after the preceding decisions |

This baseline deliberately does not introduce `validated`, `published`, `clean`, or `dirty` as formal version types.

## 7. Traceability summary

| Use case | Acceptance scenario | Principal requirements |
| --- | --- | --- |
| Collaborative editing | AC-EDIT-001 | REQ-REG-001–003, REQ-EDIT-001–003, REQ-SNAP-002, REQ-CMP-001, REQ-HIST-001–004, REQ-SEC-001–002 |
| Frequent full deliveries | AC-FULL-001 | REQ-ING-001–002, REQ-SNAP-001, REQ-MNT-001–002, REQ-RET-001–002 |
| Infrequent full replacements | AC-REPL-001 | REQ-ING-001, REQ-ING-003, REQ-SNAP-001, REQ-HIST-003, REQ-MNT-002, REQ-RET-002 |
| Supplied change batches | AC-BATCH-001 | REQ-ING-004–006, REQ-HIST-001, REQ-SEC-001–002 |
| Processed coverage outputs | AC-COV-001 | REQ-COV-001–002, REQ-ING-001, REQ-VAL-001–002 |

## 8. Evidence consulted

Accessed 2026-11-01. Documentation versions below identify the evidence reviewed and do not select the project's compatibility baseline.

| Source | Version or status | Relevance |
| --- | --- | --- |
| [PostgreSQL transactions](https://www.postgresql.org/docs/18/tutorial-transactions.html) | PostgreSQL 18 documentation (documentation home reported 18.6) | Atomic all-or-nothing behavior and visibility of intermediate transaction states |
| [PostgreSQL `COMMIT`](https://www.postgresql.org/docs/18/sql-commit.html) | PostgreSQL 18 documentation | Database transaction completion, distinct from extension-defined recorded states |
| [PostgreSQL client authentication](https://www.postgresql.org/docs/18/client-authentication.html) | PostgreSQL 18 documentation | Database connection authentication and the database role used for privileges |
| [PostgreSQL privileges](https://www.postgresql.org/docs/18/ddl-priv.html) | PostgreSQL 18 documentation | Object ownership and `GRANT`/`REVOKE` privilege boundaries |
| [PostgreSQL constraints](https://www.postgresql.org/docs/18/ddl-constraints.html) | PostgreSQL 18 documentation | Database-enforced integrity constraints and rejection behavior |
| [PostgreSQL extending SQL](https://www.postgresql.org/docs/18/extend.html) | PostgreSQL 18 documentation | Supported extension mechanisms and the still-open SQL/native split |
| [PostGIS `ST_IsValid`](https://postgis.net/docs/manual-3.6/ST_IsValid.html) | PostGIS 3.6 released manual branch | Individual 2D geometry validity |
| [PostGIS `ST_CoverageInvalidEdges`](https://postgis.net/docs/manual-3.6/ST_CoverageInvalidEdges.html) | PostGIS 3.6 released manual branch; function available since 3.4 and documented as requiring GEOS 3.12+ | Polygon coverage validity differs from individual geometry validity |
| [PostGIS Topology](https://postgis.net/docs/manual-3.6/Topology.html) | PostGIS 3.6 released manual branch | `TopoGeometry` is a separate topology model whose integration remains unresolved |
| [REQ-01 #2](https://github.com/aaliakbarian/postgis_versioning/issues/2) and [BOOT-01 #1](https://github.com/aaliakbarian/postgis_versioning/issues/1) | Repository planning state accessed 2026-11-01 | Task acceptance criteria and completed bootstrap dependency |
| [SEM-01 #4](https://github.com/aaliakbarian/postgis_versioning/issues/4) through [TECH-01 #9](https://github.com/aaliakbarian/postgis_versioning/issues/9) | Open repository planning issues accessed 2026-11-01 | Ownership of unresolved semantics, storage, SQL/security, validation, audit, and language decisions |
