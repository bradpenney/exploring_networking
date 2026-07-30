---
date: "2026-07-30 15:00"
title: "Networking Efficiency: CORS, API Gateways & ACME"
description: "Design and deploy networking with confidence — why the browser blocks cross-origin calls, what a reverse proxy or API gateway actually does, and how ACME automates TLS certificates forever."
---

# Efficiency

Essentials gets a production incident diagnosed. Efficiency is for designing the thing so it doesn't page you at 2am in the first place — the front door your API sits behind, the browser rule that trips up every back-end dev, and certificate renewal that never expires unattended again.

<div class="grid cards two-col" markdown>

-   :material-web: **[HTTP](http/cors_explained.md)**

    ---

    **[What Is CORS?](http/cors_explained.md)** — why the browser blocks cross-origin calls, and how the server actually grants permission.

-   :material-router-network: **[API Gateways](api_gateways/reverse_proxies_and_gateways.md)**

    ---

    **[Reverse Proxy vs API Gateway](api_gateways/reverse_proxies_and_gateways.md)** — how production APIs handle TLS, routing, auth, and rate limiting before traffic ever reaches your code.

-   :material-lock-check: **[TLS](tls/certificate_management.md)**

    ---

    **[ACME Protocol Explained](tls/certificate_management.md)** — how HTTP-01 and DNS-01 challenges prove domain ownership, and the automation that renews certificates forever.

</div>

More Efficiency topics — VPC design, Kubernetes networking (Services, Ingress, Network Policies), and CDN basics — are still on the way.

---

Start with **[What Is CORS?](http/cors_explained.md)** — the browser behavior that confuses more back-end engineers than anything else in this tier.
