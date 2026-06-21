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

## How It's Organized

Content is structured by **urgency and depth** — the same three tiers as the rest of the [Exploring](https://bradpenney.io/explorations) sites.

<div class="grid cards" markdown>

-   :material-package-variant: **Essentials**

    ---

    **The networking you reach for during an incident — and the mental model underneath it.**

    **[From URL to Endpoint](essentials/http/from_url_to_endpoint.md)** — What "expose an endpoint" really means: DNS, ports, and the bind address that decides who can reach it

    **[HTTPS for APIs](essentials/tls/https_for_apis.md)** — Where the connection actually gets secured, and why TLS termination changes everything

    DNS debugging, network troubleshooting, load balancers, TLS basics (coming soon)

-   :material-lightning-bolt: **Efficiency**

    ---

    **Design and operate the layer that exposes and secures your services.**

    **[Reverse Proxies and API Gateways](efficiency/api_gateways/reverse_proxies_and_gateways.md)** — The front door that owns the public address, terminates TLS, and guards your services

    **[CORS Explained](efficiency/http/cors_explained.md)** — The browser rule that makes an API "work in curl but fail in the app"

    Certificate management, advanced debugging, Services & Ingress, VPC design (coming soon)

-   :material-target: **Mastery**

    ---

    **Advanced architecture, performance, and scale.**

    Service mesh, packet capture and deep troubleshooting, network performance tuning, and multi-region networking (coming soon)

</div>

## Who This Is For

If you're the person who gets paged when an endpoint goes dark — or the one now asked to expose and secure a new one — this is for you. It assumes you're comfortable in a terminal and want to understand *why* the network behaves the way it does, not just which command to copy. No traditional networking background required; it meets you where you are.

## Part of the Exploring Series

This site is one of an integrated set of learning sites:

- **[linux.bradpenney.io](https://linux.bradpenney.io)** — Linux teaches the network commands; this site teaches when and why to use them.
- **[k8s.bradpenney.io](https://k8s.bradpenney.io)** — Kubernetes teaches Services and Ingress; this site teaches how they actually route traffic.
- **[tools.bradpenney.io](https://tools.bradpenney.io)** — the Dev Tools site teaches `curl` and `dig`; this site teaches what to send and how to read the response.
- **[cs.bradpenney.io](https://cs.bradpenney.io)** — Computer Science covers the theory behind TCP/IP, DNS, and routing.

## Getting Started

New here? Start with **[From URL to Endpoint](essentials/http/from_url_to_endpoint.md)** — once you can see how an endpoint is actually exposed, everything else about securing and debugging it falls into place.
