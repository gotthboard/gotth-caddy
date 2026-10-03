# Implementation and acceptance specification

Status: planned. All requirements below are **not implemented / not admitted**.
No endpoints, commands, package names or schema versions are stable yet.

## First vertical slice

1. Select one disposable Linux/server profile, pinned Caddy build/modules and
   storage model; record GOTTH dependency pins/license and API availability.
2. Define typed instance/grant/route/generation schemas and bounded observation
   API. Implement authenticated read-only instance/route/certificate-status UI
   through a local adapter; use real Caddy observations with redacted output.
3. Add one hostname -> approved HTTP service workflow through tests-first
   validation, preview, confirmation, durable state, serialized activation,
   readback and independent HTTP probes; preserve a second owner's route.
4. Prove denial, restart and interrupted-apply recovery before adding ACME,
   advanced routing, Stack integration or router placement. Use disposable
   certificates first; never production DNS/issuance as a test default.
5. Implement Bifrost placement only after substrate/profile admission, bounded
   resource isolation and the Caddy/keyring custody contract are proven.

## Trace and required evidence

| Requirement | Design boundary | Acceptance evidence required |
| --- | --- | --- |
| CADDY-001 | UI/API | No-JS and enhanced browser parity, keyboard/accessibility, CSRF, escaping, bounded pagination/logs and redaction |
| CADDY-002 | Placement | Exact image/binary/platform profile, install/start/stop/restart, quota and off-box profile tests |
| CADDY-003 | Scoped service API | Cross-owner/domain transfer/revocation, stale grant/generation, replay and concurrent-edit denials |
| CADDY-004 | Journal and adapter | Validation failure, second-route preservation, durable stage/install/activate/readback, timeout/unknown, crash and verified rollback |
| CADDY-005 | Routes/listeners | Admin/metadata/management SSRF, DNS rebinding, loops, tenant escape, listener conflicts and backend-bypass denials |
| CADDY-006 | TLS lifecycle | SNI/Host distinction, verified upstream TLS, renewal/expiry/DNS outage, concurrent challenges and forged-header cases |
| CADDY-007 | Credential custody | No secret exposure; compatible TLS/ACME storage, scoped use, startup/unlock, rotate/revoke, backup/recovery |
| CADDY-008 | Independent serving | UI/Stack/OIDC/database loss, Caddy restart, resource saturation, router packet/state and console recovery evidence |
| CADDY-009 | One writer | Mode handoff, old-writer fencing, drift and stale action rejection; no outage fallback writer |
| CADDY-010 | Release profile | Exact artifact/module/license matrix, upgrades/rollback, browser/API tests and matched resource/performance measurements |

Boundary cases include maximum and over-limit route counts, config/body/log
bytes, hostname/path lengths, target lists, pending operations and retry/time
budgets. Exact limits are selected before implementation, tested around each
boundary and disclosed per supported profile. No generic success/exit code
substitutes for configuration cardinality/digest and independent route probes.

## Deferred design gates

Storage engine, exact identity/session contract, stable API/schema, artifact
packaging, Caddy module allowlist and router platform support remain unselected.
Reuse `gotth-oidc` where compatible; it does not supply application sessions or
roles. Jobs/portability/release helpers are optional selected dependencies, not
a reason to require PostgreSQL on a router. Stack adapter reuse requires a real
public boundary or reviewed extraction, not importing an internal Go package.
