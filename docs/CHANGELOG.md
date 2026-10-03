# Changelog

## Unreleased

### 2026-10-02 20:35 CDT — Define GOTTH Caddy and router edge-service scope

Commit: current commit; hash assigned by Git after commit

Affected files: README.md, LICENSE, SECURITY.md, docs/PRD.md,
docs/ARCHITECTURE.md, docs/IMPLEMENTATION-SPEC.md, docs/ROADMAP.md,
docs/CHANGELOG.md.

Define a Caddy UI/service, ten requirements and traceable implementation stages.
Separate Bifrost networking, Stack orchestration, Caddy management and Caddy
serving; require single-writer ownership, restart-safe apply/rollback, scoped
routes and compatible secret custody. Include canonical/mirror reporting.

Verification: documentation/link/requirement checks and independent design
reviews are required before publication; no runtime tests are claimed.

Non-goals: no implementation, deployment, dependency install, Caddy fork,
release or native Bifrost proxy replacement. Performance is N/A for this
non-executable seed; measurements remain mandatory for implementation.
