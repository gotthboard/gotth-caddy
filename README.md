# GOTTH Caddy

**GOTTH Caddy** (`gotth-caddy`) is the planned GOTTH web UI and scoped
management service for [Caddy](https://caddyserver.com/). It manages real Caddy;
it is not a replacement proxy engine or an official upstream Caddy project.

**Design only:** no executable UI/API, image, supported release or deployment
exists yet. This repository establishes product requirements and the build plan.

## Purpose

Run Caddy as an edge service on a GOTTH Bifrost router or an independently
admitted server. Give operators a web UI for domains, reverse-proxy routes,
upstreams, TLS/certificate status, validation, preview, apply, rollback, health
and bounded redacted logs. GOTTH Stack can integrate the same service through
scoped APIs; browsers never call Caddy's Admin API directly.

- **Bifrost:** router/firewall, placement, resource and network-exposure admission.
- **Stack:** unified platform/domain panel and orchestration when enabled.
- **GOTTH Caddy:** Caddy-specific UI/API, desired route model and safe management.
- **Caddy:** HTTP(S) proxying, TLS execution and persisted serving configuration.

One admitted writer owns each Caddy instance. Stack-managed mode delegates
composition to Stack; the GOTTH Caddy UI uses that authority, not a second
independent configuration store. Standalone mode uses GOTTH Caddy's controller.
Bifrost's native `bfw-reverse-proxy` remains a separate optional component;
listener ownership is explicit and conflicts are rejected.

## Planned deployment

Selected GOTTH applications/extensions may run locally on the router where
hardware and platform admission permit; others may run off-box. Libraries are
build dependencies, not independently runnable services. This does not mean
installing every `gotth-*` project on every router.

Caddy serves approved endpoints on the router or admitted LAN services. UI,
controller and Caddy runtime are separate privilege/failure boundaries. Their
failure must not disable Bifrost packet forwarding or local console recovery.
Caddy failure can interrupt the web routes it serves; no HA is implied.

## Design and implementation path

- [Product requirements](docs/PRD.md)
- [Architecture and ownership](docs/ARCHITECTURE.md)
- [Implementation and acceptance plan](docs/IMPLEMENTATION-SPEC.md)
- [Roadmap](docs/ROADMAP.md)
- [Changelog](docs/CHANGELOG.md)

Use Go/templ, HTMX and Tailwind, with essential server-rendered workflows.
Reuse admitted GOTTH identity and shared mechanisms; missing libraries or
private Stack adapters are not claimed to be available public dependencies.

## Source and reporting

Private [Forgejo](https://git.dannyhunn.com/gotthboard/gotth-caddy) is canonical.
[GitHub](https://github.com/gotthboard/gotth-caddy) is the public one-way mirror.
Use [GitHub Issues](https://github.com/gotthboard/gotth-caddy/issues) for bugs and
[private vulnerability reporting](https://github.com/gotthboard/gotth-caddy/security/advisories/new)
for security issues; see [SECURITY.md](SECURITY.md). MIT applies to this repository;
Caddy and all dependencies retain their own licenses and notices.
