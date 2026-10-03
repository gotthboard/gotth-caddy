# GOTTH Caddy product requirements

Status: defined, implementation and runtime acceptance pending.

## Users and scope

Platform administrators manage admitted Caddy instances and global policy.
Domain owners manage only assigned hostnames and approved service targets.
Operators need a useful Caddy UI on a router, not access to raw host authority.

- **CADDY-001:** Provide Go/templ server-rendered UI/API for instance inventory,
  domains, host/path routes, upstreams, TLS/certificate metadata, health and
  bounded redacted logs. Essential tasks work without JavaScript; HTMX is
  progressive enhancement, with keyboard/accessibility and CSRF protections.
- **CADDY-002:** Run independently or as an admitted Bifrost-hosted edge service;
  offer scoped Stack integration without requiring Stack for established traffic.
  Declare exact CPU/OS/runtime/artifact, resource, storage and recovery support.
- **CADDY-003:** Enforce platform-admin versus domain-owner grants server-side.
  Bind each operation to actor, instance, ownership/grant revision, canonical
  hostname/matcher, approved target and configuration generation. Domain transfer
  or revocation invalidates pending actions and cached authority.
- **CADDY-004:** Validate, preview and confirm complete configurations; serialize
  mutations under one fenced writer per instance. Preserve unrelated routes,
  durably record intent, verify runtime and restart state, and support verified
  rollback. Timeouts yield unknown/reconciliation-required, never assumed success.
- **CADDY-005:** Keep Caddy Admin API private and available only to its admitted
  controller adapter. Never publish it, the router management plane, metadata
  endpoints, other tenants or arbitrary caller-selected upstreams. Deny proxy
  cycles, listener conflicts, forged forwarded headers and DNS-rebinding escapes.
- **CADDY-006:** Assign exactly one certificate lifecycle owner per termination
  point. Show redacted issuance/renewal/expiry state; scope DNS-01 grants and
  preserve concurrent challenges. Separate public Host/scheme from upstream
  TLS identity, verify cross-host TLS, and bound outage/expiry behavior.
- **CADDY-007:** Keep credentials out of UI, logs, ordinary config and exports.
  Bifrost hosting uses admitted keyring-mediated use; Caddy TLS private-key,
  ACME-account and module credential storage needs an explicit compatible
  custody/lease design before admission, not an assumed opaque-key capability.
- **CADDY-008:** Management/UI/identity/database outage denies affected mutations
  but leaves Caddy serving its last valid configuration and permits independent
  restart/local recovery. Caddy failure may break hosted HTTP(S), not router
  forwarding. Resource saturation must respect reserved router headroom.
- **CADDY-009:** Stack-managed mode has Stack as sole composition authority;
  standalone mode has GOTTH Caddy as sole authority. No concurrent writers.
  Mode migration requires fenced handoff, exact state verification and rollback.
- **CADDY-010:** Pin Caddy, allowed modules, toolchain and GOTTH dependencies;
  prove lifecycle, isolation, browser/API parity, upgrade/recovery and performance
  per deployment profile. Catalog membership or upstream tests admit nothing.

## Non-goals for the first implementation

No router/firewall/routing authority, arbitrary Caddyfile editor for tenants,
plugin marketplace, on-demand module builds, public Admin API, shell execution,
mail-protocol proxying or automatic deployment. No reimplementation of Caddy,
Caddy fork, replacement of Bifrost's native proxy, or promise of universal
cross-platform support. A database and container runtime are not selected yet.
