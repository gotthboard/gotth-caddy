# Roadmap

All stages are planned; none is complete runtime work.

1. **Contracts:** finish exact instance/route/grant, storage, Caddy build,
   credential and deployment-profile choices from the implementation spec.
2. **Read-only UI:** authenticated instance, route, health and certificate
   metadata views, usable without JavaScript.
3. **One safe route:** preview -> confirm -> apply -> verify -> rollback;
   prove restart, concurrent owners and interrupted-operation recovery.
4. **TLS and operations:** admitted ACME/DNS lifecycle, redacted logs, health,
   upgrades and protected recovery, with measured resource bounds.
5. **Stack and router:** prove single-writer integration and Bifrost-hosted
   edge profile, resource/traffic isolation and off-box service targets.

Do not deploy or declare a router-ready release from documentation, mocks,
upstream tests or the existence of the public repository.
