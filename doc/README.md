# postgis_versioning documentation

> **Draft:** This documentation is awaiting maintainer review under [REQ-01](https://github.com/aaliakbarian/postgis_versioning/issues/2).

This directory separates product requirements, architecture decisions, research, and development guidance so that proposals and findings do not become accepted requirements by accident.

## Project documents

| Document | Purpose | Status |
| --- | --- | --- |
| [Product vision](project/vision.md) | Product intent, boundaries, and long-term direction | Draft for maintainer review |
| [Product requirements](project/requirements.md) | Stable requirements, acceptance scenarios, recommendations, and open decisions | Draft for maintainer review |

## Documentation areas

| Area | Content |
| --- | --- |
| `project/` | Accepted product context, requirements, and release boundaries |
| `architecture/decisions/` | Reviewed architecture decisions and their consequences |
| `architecture/research/` | Evidence and research findings that inform decisions |
| `development/` | Reproducible development, build, and test instructions |

## Status language

- **Accepted boundary:** supplied project scope that this draft treats as fixed unless the maintainer changes it.
- **Derived requirement:** required behavior inferred directly from an accepted use case; acceptance still depends on maintainer review of this draft.
- **Expressed goal:** a desired outcome whose guarantees and mechanisms remain unresolved.
- **Agreed preference:** an expressed direction whose precise contract or feasibility may remain unresolved.
- **Recommendation:** a proposed direction that requires explicit approval.
- **Open decision:** an unresolved question owned by later design or research work.

Technical claims should cite version-matched PostgreSQL or PostGIS documentation. A version used as evidence is not automatically the project's compatibility baseline.
