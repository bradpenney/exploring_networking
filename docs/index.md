---
date: "2026-06-21 12:00"
title: Exploring Networking - Networking for SREs and Platform Engineers
description: "Practical networking for SREs and platform engineers — how APIs get exposed and secured on the wire, plus DNS, load balancers, TLS, and Kubernetes networking."
---

# Exploring Networking

**Debug faster. Route correctly. Keep services connected.**

<img src="images/exploring_networking.png" alt="Exploring Networking" class="img-responsive-right" width="300">

Welcome to a practical guide to the networking that production actually runs on — the DNS lookups, load balancers, certificates, and routes that keep services reachable, and the diagnostics that get them back when they break.

## The Problem

It's 2am. The API is timing out. Users are getting 502s from the load balancer. You SSH in and... where do you even start? Is it DNS? A firewall rule? Latency? An expired TLS certificate?

**Traditional networking courses start with the OSI model and packet-tracer simulations. This site starts with "How do I fix this production outage right now?"**

## Start Here: How APIs Work on the Wire

The first published series tackles the question every app admin and platform engineer eventually hits — *how is an API endpoint actually exposed, and how is it secured?* Read it in this order:

<div class="grid cards" markdown>

-   :material-numeric-1-circle: **[From URL to Endpoint](http/essentials/from_url_to_endpoint.md)**

    ---

    What "expose an endpoint" really means: DNS, ports, listening processes, and the bind address that decides who can reach it.

-   :material-numeric-2-circle: **[HTTPS for APIs](tls/essentials/https_for_apis.md)**

    ---

    Where the connection actually gets secured — and why TLS termination means encryption rarely stops where you think.

-   :material-numeric-3-circle: **[Reverse Proxies and API Gateways](api_gateways/efficiency/reverse_proxies_and_gateways.md)**

    ---

    The front door that owns the public address, terminates TLS, routes traffic, and guards your services.

-   :material-numeric-4-circle: **[CORS Explained](http/efficiency/cors_explained.md)**

    ---

    The browser rule that makes an API "work in curl but fail in the app" — and the server header that fixes it.

</div>

## Coming Soon

More of the site is in active development. These categories are being written and will publish here as they're finished:

- **DNS Debugging** — `dig`, `nslookup`, and why DNS is the usual suspect
- **Network Troubleshooting** — `ping`, `traceroute`, `mtr`, and `curl` as an incident workflow
- **Load Balancers** — health checks, 502/503/504, and traffic distribution
- **TLS & Certificates** — the handshake, the chain of trust, and renewal automation
- **Kubernetes Networking** — Services, Ingress, and how traffic reaches a Pod
- **VPC Design** — CIDR planning, subnets, and routing that scales

## Part of the Exploring Series

This site is one of an integrated set of learning sites:

- **[linux.bradpenney.io](https://linux.bradpenney.io)** — Linux teaches the network commands; this site teaches when and why to use them.
- **[k8s.bradpenney.io](https://k8s.bradpenney.io)** — Kubernetes teaches Services and Ingress; this site teaches how they actually route traffic.
- **[tools.bradpenney.io](https://tools.bradpenney.io)** — the Dev Tools site teaches `curl` and `dig`; this site teaches what to send and how to read the response.
- **[cs.bradpenney.io](https://cs.bradpenney.io)** — Computer Science covers the theory behind TCP/IP, DNS, and routing.

---

**New here?** Start with **[From URL to Endpoint](http/essentials/from_url_to_endpoint.md)** — once you can see how an endpoint is exposed, everything else about securing and debugging it falls into place.
