---
date: "2026-06-21 12:00"
title: "Reverse Proxies and API Gateways, Demystified"
description: "Production APIs sit behind a front door. Learn how reverse proxies and API gateways handle TLS, routing, auth, and rate limiting before traffic hits you."
---

# Reverse Proxies and API Gateways, Demystified

!!! tip "Part of a Learning Path"
    This article is part of the [How APIs Actually Work](https://bradpenney.io/pathways/how-apis-work) pathway on [bradpenney.io](https://bradpenney.io) — a guided sequence through the topic. It also stands on its own.

You've now [exposed an endpoint](../../essentials/http/from_url_to_endpoint.md) and [secured it with HTTPS](../../essentials/tls/https_for_apis.md). But in production, clients almost never connect to your application directly. There's a piece of infrastructure standing in front of it — the thing that actually owns the public address, terminates TLS, and decides which requests even reach your code. When someone says "the endpoint is behind the gateway," *this* is what they mean, and not understanding it leaves a permanent gap in the picture of how an API is exposed and secured.

This article demystifies that front door: the reverse proxy and its more capable cousin, the API gateway.

## The Core Idea: A Front Door for Your Services

A **reverse proxy** is a server that sits between clients and your backend services. Clients talk to *it*; it talks to your services on their behalf and relays the responses back. From the client's perspective, the proxy *is* the API — the real servers are invisible.

```mermaid
flowchart TD
    C1[Client] --> P[Reverse Proxy / Gateway<br/>the only public endpoint]
    C2[Client] --> P
    P -->|/orders| S1[Orders service]
    P -->|/users| S2[Users service]
    P -->|/billing| S3[Billing service]
    style C1 fill:#1a202c,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style C2 fill:#1a202c,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style P fill:#326CE5,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style S1 fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style S2 fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style S3 fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
```

This flips the exposure model from the [direct-binding picture](../../essentials/http/from_url_to_endpoint.md): your services bind to a *private* address, unreachable from the internet, and only the proxy is bound to the public port. The proxy becomes your single, defensible front door — which is exactly why production systems are built this way.

!!! tip "Forward proxy vs reverse proxy"

    A *forward* proxy sits in front of **clients** and hides them from servers (corporate web filters, VPNs). A *reverse* proxy sits in front of **servers** and hides them from clients. Same idea, opposite direction — and APIs are about reverse proxies.

## What the Front Door Does For You

Putting a proxy in front lets you handle cross-cutting concerns *once*, at the edge, instead of reimplementing them in every service.

<div class="grid cards" markdown>

-   :material-lock: __TLS Termination__

    ---

    **Why it matters:** one certificate at the edge instead of one per service.

    The proxy [terminates TLS](../../essentials/tls/https_for_apis.md), so backends don't each manage certs. The most common reason a front door exists.

-   :material-sign-direction: __Routing__

    ---

    **Why it matters:** many services behind one address.

    `/orders` goes to the orders service, `/users` to users — path- or host-based rules map one public endpoint onto many private ones.

-   :material-scale-balance: __Load Balancing__

    ---

    **Why it matters:** spread traffic, survive a dead instance.

    The proxy distributes requests across healthy backends and stops sending to ones that fail health checks.

-   :material-shield-account: __Auth & Rate Limiting__

    ---

    **Why it matters:** reject bad traffic before it costs you.

    Validate tokens and enforce quotas at the edge so invalid or abusive requests never reach your code.

</div>

## Reverse Proxy vs API Gateway: Where's the Line?

These terms overlap, and people use them loosely. The useful distinction is *how much application-awareness* the front door has.

| | Reverse Proxy | API Gateway |
| :--- | :--- | :--- |
| **Primary job** | Relay and balance traffic | Manage APIs as products |
| **Operates on** | Connections, hosts, paths | Requests, routes, identities, quotas |
| **Typical features** | TLS, routing, load balancing, caching | All that **plus** auth, rate limiting, API keys, request/response transformation, usage analytics |
| **Examples** | Traefik, HAProxy, Envoy, NGINX | Kong, Traefik Hub, AWS API Gateway, Apigee, Envoy + control plane |

The short version: **an API gateway is a reverse proxy that understands your API**. A plain reverse proxy moves bytes to the right backend. A gateway additionally knows what a "request," a "consumer," and a "rate limit" are, and enforces policy on them. Many tools (Envoy especially) can be either, depending on configuration.

You reach for a gateway specifically when you want to centralize *API-level policy* — authentication, per-consumer quotas, key management — rather than bake it into every service.

## A Minimal Reverse Proxy Config

Concretely, here's [Traefik](https://traefik.io/) terminating TLS and routing two paths to two private backends. Traefik splits its config in two: a small *static* part defines the entrypoints (ports), and a *dynamic* part defines the routing — this is the dynamic half:

```yaml title="Traefik (file provider): a routing, TLS-terminating front door" linenums="1"
http:
  routers:
    orders:
      rule: "Host(`api.example.com`) && PathPrefix(`/orders`)"   # (1)!
      entryPoints: [websecure]                                   # (2)!
      service: orders
      tls: {}                                                    # (3)!
    users:
      rule: "Host(`api.example.com`) && PathPrefix(`/users`)"
      entryPoints: [websecure]
      service: users
      tls: {}

  services:
    orders:
      loadBalancer:
        servers:
          - url: "http://10.0.1.10:8080"                         # (4)!
    users:
      loadBalancer:
        servers:
          - url: "http://10.0.1.11:8080"
```

1. Host- and path-based routing: a request for `api.example.com/orders` matches this router.
2. `websecure` is the port-443 entrypoint, declared once in Traefik's static config — the proxy, not the app, binds the public port and speaks TLS.
3. `tls: {}` terminates TLS here, at the front door. Traefik can fetch and auto-renew the certificate from an ACME/Let's Encrypt resolver, so the cert lives at the termination point, never in your services.
4. Forwarded to a **private** IP/port the internet can't reach directly. Traefik sets the `X-Forwarded-*` headers (including `X-Forwarded-Proto`) automatically, so the [redirect loop](../../essentials/tls/https_for_apis.md) that bites hand-rolled proxies never happens here.

A gateway layers policy on top of this — authentication, rate limiting, request/response transformation — added as **middlewares** chained onto a router, but the routing skeleton is the same.

!!! danger "Trusting forwarded headers"

    The proxy sets `X-Forwarded-For` / `X-Forwarded-Proto` so the backend can see the *original* client. But the gateway must **strip or overwrite** any such header arriving from an untrusted client — if it blindly appends, a caller can forge `X-Forwarded-For` and spoof their source IP, defeating IP allowlists, rate limits, and audit logs. Trust these headers only from proxies you control.

## Where Authentication and Authorization Land

This is the part that ties back to the security model and trips people up. A gateway can **authenticate** — validate a token's signature and reject `401`s before they ever touch your services. That's a genuine win: invalid traffic dies at the edge.

But a gateway generally **cannot authorize** your business rules, because it doesn't know them. "Is this a valid token for user 42?" — the gateway can answer. "Does user 42 own order 88?" — only your service knows. So the durable pattern is:

- **Authentication** at the gateway (centralized, consistent).
- **Authorization** in the service (it owns the data and the rules).

One useful refinement: *coarse* authorization can live at the edge too. "Is the caller in an allowed group?" is a membership check a proxy can make; it's *business-rule* authorization ("does user 42 own order 88?") that it can't. A sidecar auth proxy like [forevd](https://github.com/firestoned/forevd) sits exactly here — it externalizes authentication (mTLS or OIDC) *and* coarse authorization (LDAP group or user allowlists) out of your application code, leaving the fine-grained, data-dependent checks to the service. It's one of several tools (alongside full gateways and service meshes) in this space.

!!! info "Disclosure"
    I contribute to forevd. It's mentioned here as one example of the sidecar auth pattern — not an endorsement.

Assuming the gateway did *both* is a classic way over-permissive APIs ship. The full reasoning lives in the CS-side [authentication vs authorization](https://cs.bradpenney.io/efficiency/web/authentication_vs_authorization/) article; the networking takeaway is simply: **the front door checks identity; the service checks permission.**

## Why This Matters for Platform Work

- **It's where most production API config actually lives.** Routing, TLS, rate limits, and the public DNS record point at the gateway. When an endpoint misbehaves, the gateway is the first place to look — often before the application.
- **It shrinks your attack surface to one defensible door.** Backends bind privately; only the gateway is exposed. You harden, monitor, and patch one front door instead of a dozen services.
- **It explains a whole family of 5xx errors.** [`502`/`503`/`504`](https://cs.bradpenney.io/efficiency/web/anatomy_of_request_response/) usually originate at the proxy when it can't reach or get a timely response from a backend — the app may be the cause, but the *messenger* is the gateway, and reading its logs is how you tell them apart.

## Common Scenarios

=== ":material-numeric-5-box: 502 Bad Gateway"

    The client gets `502`, but your app logs show no incoming request. The **proxy** couldn't reach the backend (wrong upstream address, backend crashed, or failing health checks so the proxy pulled it from rotation). `502` literally means "I'm a gateway and the server behind me gave me a bad/no response." Check the proxy's upstream config and the backend's health, not the application logic first.

=== ":material-timer-alert: 504 Gateway Timeout"

    The backend *is* reachable but too slow — it didn't respond within the proxy's timeout. Either the request is genuinely slow (optimize the backend, or it's calling slow downstream services) or the proxy timeout is set too aggressively. `504` points at latency; `502` points at reachability.

=== ":material-account-cancel: 401 at the edge, app never sees it"

    A request with a bad token returns `401` instantly and your service logs nothing. That's the gateway **authenticating** and rejecting before forwarding — working as intended. If you need the service to make finer-grained decisions, ensure the gateway forwards the verified identity (e.g., in a trusted header) so the service can still **authorize**.

## Practice Problems

??? question "Practice Problem 1: Reverse or Forward?"

    Your company routes all employee web browsing through a server that filters and logs outbound traffic. Is that a forward proxy or a reverse proxy? How is it different from the thing in front of your API?

    ??? tip "Solution"

        It's a **forward proxy** — it sits in front of the *clients* (employees) and mediates their access to the outside world, hiding/representing the clients. The thing in front of your API is a **reverse proxy** — it sits in front of the *servers*, representing them to incoming clients and hiding the real backends. Same mechanism (an intermediary that relays traffic), opposite direction depending on whether it fronts clients or servers.

??? question "Practice Problem 2: 502 vs 504"

    Two incidents: in the first, clients get `502 Bad Gateway`; in the second, `504 Gateway Timeout`. Your application is the suspect in both. What does each code tell you about where to look?

    ??? tip "Solution"

        **`502`** means the proxy reached (or tried to reach) the backend but got a **bad or no response** — the backend is down, crashed, returned garbage, or was pulled from rotation by failed health checks. Look at whether the backend is *up and reachable from the proxy*. **`504`** means the backend was reachable but **didn't respond in time** — it's a **latency/timeout** problem. Look at backend response times (and any slow downstream calls it makes) or whether the proxy's timeout is too tight. Reachability vs slowness — the code tells you which.

??? question "Practice Problem 3: Who Authorizes?"

    Your platform team enables JWT validation on the API gateway. A developer removes all permission checks from their service, reasoning "the gateway handles auth now." What breaks, and when?

    ??? tip "Solution"

        Nothing breaks *immediately* — which is what makes it dangerous. The gateway handles **authentication** (it confirms the token is valid and identifies the caller), so logged-in users still get through. But it does **not** handle **authorization** of business rules, because it doesn't know which user may access which resource. With the service's permission checks gone, **any authenticated user can now access any other user's data** — the most common API breach. The gateway checks *identity*; the service must still check *permission*.

## Key Takeaways

| Concept | What It Means |
| :--- | :--- |
| **Reverse proxy** | A front door that relays client traffic to private backends |
| **API gateway** | A reverse proxy that also understands and enforces API-level policy |
| **Single public endpoint** | Backends bind privately; only the proxy is exposed |
| **Cross-cutting concerns** | TLS, routing, load balancing, auth, rate limiting — handled once at the edge |
| **AuthN vs authZ split** | Gateway authenticates (identity); service authorizes (permission) |
| **5xx attribution** | `502` = backend unreachable/bad response; `504` = backend too slow |

The front door is the part of "how an API is exposed and secured" that's invisible from the client and central to the design. Once you see that backends hide on a private network while a single proxy owns the public address — terminating TLS, routing paths, balancing load, and authenticating callers — the production picture finally closes. The endpoint isn't exposed to the world; the *gateway* is, and it decides what gets through.

## Further Reading

### Related Networking Articles

- **[From URL to Endpoint](../../essentials/http/from_url_to_endpoint.md)** — the direct-exposure model the gateway replaces.
- **[HTTPS for APIs: Where the Connection Gets Secured](../../essentials/tls/https_for_apis.md)** — TLS termination, which the gateway usually performs.
- **[Load Balancer Basics](../../essentials/load_balancers/load_balancer_basics.md)** — health checks and distribution, a core gateway job.

### Computer Science Fundamentals

- **[Authentication vs Authorization (cs.bradpenney.io)](https://cs.bradpenney.io/efficiency/web/authentication_vs_authorization/)** — why the gateway can authenticate but not authorize.
- **[Anatomy of an HTTP Request and Response (cs.bradpenney.io)](https://cs.bradpenney.io/efficiency/web/anatomy_of_request_response/)** — the `5xx` status codes gateways return.

### External Resources

- [Traefik Proxy documentation](https://doc.traefik.io/traefik/) — the reverse proxy used in this article's example.
- [F5/NGINX: What is a reverse proxy?](https://www.f5.com/glossary/reverse-proxy) — the canonical concept explainer.
- [Cloudflare: API gateway](https://www.cloudflare.com/learning/security/api/what-is-an-api-gateway/) — gateway-specific capabilities.
- [Kong Gateway documentation](https://developer.konghq.com/gateway/) — a widely-used open-source API gateway.
