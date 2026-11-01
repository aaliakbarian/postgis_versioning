# postgis_versioning documentation

> **Reviewed requirements baseline:** Accepted by the maintainer on 2026-11-01 under [REQ-01 (#2)](https://github.com/aaliakbarian/postgis_versioning/issues/2) as the foundation for Phase 1 design.

This directory separates product requirements, architecture decisions, research, and development guidance so that proposals and findings do not become accepted requirements by accident.

## Project documents

| Document | Purpose | Status |
| --- | --- | --- |
| [Product vision](project/vision.md) | Product intent, boundaries, and long-term direction | Reviewed requirements baseline |
| [Product requirements](project/requirements.md) | Stable requirements, acceptance scenarios, recommendations, and open decisions | Reviewed requirements baseline |

## Documentation areas

| Area | Content |
| --- | --- |
| `project/` | Accepted product context, requirements, and release boundaries |
| `architecture/decisions/` | Reviewed architecture decisions and their consequences |
| `architecture/research/` | Evidence and research findings that inform decisions |
| `development/` | Reproducible development, build, and test instructions |

## Status language

- **Accepted boundary:** accepted product scope in this baseline unless the maintainer changes it through later review.
- **Derived requirement:** accepted product requirement in this baseline; detailed semantics and initial-release inclusion remain governed by recorded open work.
- **Expressed goal:** a desired outcome whose guarantees and mechanisms remain unresolved.
- **Agreed preference:** an expressed direction whose precise contract or feasibility may remain unresolved.
- **Recommendation:** a proposed direction that requires explicit approval.
- **Open decision:** an unresolved question owned by later design or research work.

Baseline acceptance applies to the documented product boundaries, derived requirements, and acceptance scenarios. Expressed goals, agreed preferences with unresolved mechanisms, open decisions, and recommendations retain their recorded individual status.

Technical claims should cite version-matched PostgreSQL or PostGIS documentation. A version used as evidence is not automatically the project's compatibility baseline.
