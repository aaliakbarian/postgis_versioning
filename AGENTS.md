# Repository working instructions

## Scope and evidence

- Inspect existing files and relevant documentation before editing.
- Keep changes focused on the requested issue.
- Put product documentation in `doc/`: project material in `doc/project/`, decisions in `doc/architecture/decisions/`, research in `doc/architecture/research/`, and development guidance in `doc/development/`.
- Keep accepted requirements, proposals, research findings, and unresolved questions distinct.
- Verify important technical claims against version-matched PostgreSQL and PostGIS documentation and maintained project sources.

## Product boundaries

- Treat independent per-table versioning of PostGIS `geometry` columns and relational attributes, with SQL as the primary interface, as the initial scope.
- Keep application authentication, business approvals, REST services, raster, object-storage management, map serving, and initial Citus-specific implementation external to this project.
- Treat `geography` as deferred and native `TopoGeometry` integration as unresolved.
- C versus Rust/pgrx, compatibility versions, SQL signatures, state-recording boundaries, storage layout, delta-finalization guarantees, and retention exceptions remain unresolved.
- Do not turn unresolved items into implementation decisions during repository bootstrap.

## Working conventions

- Prefer containerized compilation and database or testing dependencies; keep host installations minimal.
- Inspect available tools, versions, images, and repository configuration before relying on them.
- Use appropriate installed Superpowers workflows while respecting explicit user instructions.
- Keep operational instructions portable and repository-relative.
- Run verification appropriate to the change, and report exact commands, results, limitations, and unresolved concerns.
- Never stage, commit, push, create branches or pull requests, mutate GitHub planning, or install dependencies unless explicitly requested.
