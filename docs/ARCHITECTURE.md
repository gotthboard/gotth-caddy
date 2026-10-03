# GOTTH Caddy architecture

Status: planned, no runtime admitted.

## Planes and ownership

```text
operator -> GOTTH Caddy UI / GOTTH Stack domain panel
         -> authenticated scoped service API
         -> selected composition authority + durable operation journal
         -> local Caddy adapter -> private Caddy Admin API

Internet -> admitted Bifrost listener/firewall -> Caddy
                                              -> local GOTTH service
                                              -> approved LAN service
```

Bifrost alone admits host network changes through its core/domain providers.
GOTTH Caddy requests placement and listener bindings; it cannot program NAT,
routes, firewall rules, host files or service processes on its own authority.
The Bifrost profile retains `rpc-plugin-system` and the admitted keyring,
filesystem and execution providers. Packaging is not permission. No raw Docker
socket, host-network or privileged-container shortcut is implied.

Libraries such as `gotth-oidc` and `gotth-portability` are consumed in services;
products/extensions are deployable only after their own runtime admission.
An edge host need not run Stack, identity, PostgreSQL or all GOTTH products.

## Exactly one writer

Standalone: GOTTH Caddy controller owns desired Caddy state and composition.
Stack-managed: Stack owns desired composition and journal; GOTTH Caddy is its
scoped UI/API facade and Caddy-specific integration surface. Mutating UI
requests go through Stack and fail closed while it is unavailable. Read-only
observations remain labelled with freshness. There is no fallback writer.

The existing Stack Caddy adapter is private, not an importable public service.
Reuse requires either a scoped Stack-owned API or a separately reviewed upstream
extraction preserving its complete-file, no-import/no-environment-expansion,
fixed-binary and loopback restrictions. Do not copy a competing controller.
Implementation must reconcile the exact Stack revision; older local design
branches do not imply a published API or an implemented apply path.

Writer changes require revoking old grants, fencing old process/generation,
reconciling desired/file/runtime state and transferring authority explicitly.
Version checks in GOTTH do not grant Caddy native multi-request transactions.

## Safe service and route model

A binding contains instance and owner/grant revision, normalized hostname and
path rules, reserved listener/address/port, approved service identity, transport,
upstream TLS name/trust policy, certificate owner and resource profile.
Reject exact/wildcard conflicts, cross-owner access, proxy loops, management and
metadata addresses, redirects to forbidden targets, and DNS changes that escape
an admitted destination. Explicitly granted LAN targets are supported; private
address space alone grants nothing. For private backends, firewall/listener
isolation must prevent clients bypassing the intended proxy.

Caddy and `bfw-reverse-proxy` cannot both own one address/port. Default is direct
Caddy-to-service routing. A nested product proxy is optional with an explicit
trust/ownership contract, never automatic. Forwarded headers are stripped or
normalized unless from an authenticated/admitted proxy hop. Cross-host hops
verify TLS; local HTTP requires an explicitly admitted local transport boundary.

## Apply, durability and recovery

Use complete, deterministic, bounded configuration and an operation journal.
Keep desired state, persistent startup configuration and active Caddy JSON
separate. Preflight observes all three and rejects unexplained drift. Stage
previous/candidate state durably, install startup state, activate, inspect and
verify required routes independently, then record completion. Unknown results
require observation before retry or rollback. Preserve other owners' routes.

Caddy documents atomic replacement for `POST /load` and rollback on loading
failure, not a transaction spanning GOTTH state, files, DNS and network policy.
`POST /adapt` is conversion, not runtime acceptance. Provisioning validation
can have effects and belongs in a bounded admitted environment. Warning and
partial-result policy must be explicit, not silently interpreted as success.

Choose exactly one supported startup source per profile: persistent configuration
file, or explicitly admitted autosave/resume. Prove restart from it. Do not
assume API changes survive a service that starts from an unchanged Caddyfile.
The initial Stack-compatible adapter uses its existing file-based startup model.
Keep its Admin endpoint unchanged through candidate and rollback.

## Secrets and failure domains

Caddy normally needs durable access to TLS and ACME private material. This is
an open implementation gate for the Bifrost profile: select a compatible storage
integration and least-privilege credential use, including restart/unlock,
rotation, backup, revoke and recovery. Do not claim stock Caddy uses Bifrost
opaque keyring handles. The UI displays metadata only; secret-free exports are
separate from protected recovery backups.

Caddy's serving process remains independent of UI/controller availability.
Existing HTTP routes continue while valid config/keys/certificates and upstreams
permit; renewal and expiry remain real limits. Hosting on a router shares a
hardware failure domain; no HA or indefinite availability claim is made.

## Source contracts

- [Caddy API](https://caddyserver.com/docs/api): load, inspect, adaptation,
  optimistic concurrency and autosave/resume semantics (reviewed 2026-10-02).
- [Caddy command line](https://caddyserver.com/docs/command-line): validation,
  reload and startup modes; exact binary/modules must be pinned in implementation.
- [Bifrost edge-hosting contract](https://github.com/gotthboard/gotth-bifrost/blob/main/documents/EDGE-SERVICES.md).
