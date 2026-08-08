---
date: "2026-07-30 15:00"
title: "Networking Essentials: DNS, TLS & Load Balancers"
description: "The networking every SRE and platform engineer needs to debug production today — how a URL becomes a live endpoint, DNS resolution, TLS handshakes, and load balancer health checks."
---

# Essentials

The API is returning 502s. Is it the load balancer, DNS, or the app itself? Essentials is the networking layer underneath that question — not academic theory, the specific mechanics you need when something between the client and your service is broken.

<div class="grid cards two-col" markdown>

-   :material-web: **[HTTP](http/from_url_to_endpoint.md)**

    ---

    **[How an API Endpoint Gets Exposed](http/from_url_to_endpoint.md)** — following a URL through DNS, ports, and the listening server to see where your API actually lives.

-   :material-lock: **[TLS](tls/tls_basics.md)**

    ---

    **[TLS Handshake Explained](tls/tls_basics.md)** and **[TLS Termination & mTLS for APIs](tls/https_for_apis.md)** — certificates, the chain of trust, and where the encrypted connection actually stops.

-   :material-dns: **[DNS](dns/how_dns_works.md)**

    ---

    **[DNS Explained](dns/how_dns_works.md)** — resolvers, records, and TTLs. DNS doesn't "propagate," caches expire; know the difference before your next deploy day.

-   :material-scale-balance: **[Load Balancers](load_balancers/load_balancer_basics.md)**

    ---

    **[Load Balancer Explained](load_balancers/load_balancer_basics.md)** — how traffic gets distributed across servers, health checks, and why L4 vs L7 changes everything.

-   :material-transit-connection-variant: **[Tunneling](tunneling/ssh_tunnels.md)**

    ---

    **[SSH Tunnels Explained](tunneling/ssh_tunnels.md)** — local/remote port forwarding, dynamic SOCKS proxying, and `ProxyJump`, from the one idea underneath all four.

</div>

More Essentials topics — DNS debugging tools and general network troubleshooting (`ping`, `traceroute`, `curl`) — are still on the way.

---

Start with **[How an API Endpoint Gets Exposed](http/from_url_to_endpoint.md)** — the request/response path every other article in this tier plugs into.
