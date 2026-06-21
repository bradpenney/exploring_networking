# Exploring Networking

**Debug faster. Route correctly. Keep services connected.**

A practical guide to networking for SREs and Platform Engineers. Part of the [BradPenney.io](https://bradpenney.io) learning ecosystem.

**Live Site:** [networking.bradpenney.io](https://networking.bradpenney.io)

## Overview

It's 2am. The API is timing out. Users are getting 502 errors from the load balancer. You SSH into the server and... where do you even start? Is it DNS? A firewall rule? Network latency? The TLS certificate?

**Traditional networking courses start with the OSI model and packet tracer simulations. This site starts with "How do I fix this production outage RIGHT NOW?"**

### The Practical Approach

While networking textbooks teach protocol theory and subnetting math, this site teaches what SREs and Platform Engineers actually need:

- Debug network issues 10x faster with the right tools
- Understand load balancers, DNS, and TLS in production contexts
- Design VPCs and security groups that actually work
- Troubleshoot Kubernetes networking (Services, Ingress, Network Policies)
- Make informed decisions about CDNs, service mesh, and network architecture

**No OSI model memorization. No packet tracer. Just practical, incident-driven networking knowledge.**

### Who This Is For

**SREs and Platform Engineers** who:

- Respond to incidents involving network timeouts, 502 errors, DNS failures
- Manage cloud networking (AWS VPCs, GCP networks, Azure VNets)
- Deploy applications to Kubernetes and debug connectivity issues
- Configure load balancers, TLS certificates, and CDNs
- Need to make networking decisions under pressure

You may or may not have a traditional networking background. You might be a former sysadmin learning cloud networking, a developer transitioning to platform work, or a DevOps engineer managing infrastructure. **This site is for you.**

## Site Structure

Content is organized by **urgency and job context**, not academic progression:

### 📦 Essentials (Debug Production Issues Today)

- **DNS Debugging** - dig, nslookup, /etc/resolv.conf, why DNS always breaks
- **Network Troubleshooting** - ping, traceroute, mtr, curl basics
- **Firewall Basics** - Security groups, iptables, why traffic is blocked
- **Load Balancers** - Health checks, 502 errors, sticky sessions

### ⚡ Efficiency (Design and Deploy Confidently)

- **TLS/SSL Certificates** - Let's Encrypt, cert-manager, certificate renewal
- **VPC Design** - CIDR blocks, routing tables, subnet strategy
- **Kubernetes Networking** - Services, Ingress, Network Policies
- **CDN Basics** - CloudFlare, CloudFront, caching strategies

### 🎯 Mastery (Advanced Architecture and Performance)

- **Service Mesh** - Istio/Linkerd basics for traffic management
- **Advanced Troubleshooting** - tcpdump, Wireshark, eBPF tools
- **Network Performance** - Latency optimization, bandwidth tuning
- **Multi-Region Networking** - Cross-region traffic, disaster recovery

**No arbitrary levels.** Pick what you need when you need it. Essentials solve today's incidents. Efficiency prevents tomorrow's. Mastery optimizes everything.

## Project Structure

```
exploring_networking/
├── docs/                           # Markdown content organized by topic
│   ├── dns/                        # DNS and name resolution
│   │   ├── essentials/             # dig, nslookup, common DNS issues
│   │   └── efficiency/             # DNS architecture, Route53, records
│   ├── troubleshooting/            # Network debugging
│   │   ├── essentials/             # ping, traceroute, mtr, curl
│   │   ├── efficiency/             # ss, netstat, advanced debugging
│   │   └── mastery/                # tcpdump, Wireshark, eBPF
│   ├── load_balancers/             # Load balancing
│   │   ├── essentials/             # Health checks, 502 errors
│   │   └── efficiency/             # ALB, NLB, HAProxy, NGINX
│   ├── tls/                        # TLS/SSL certificates
│   │   ├── essentials/             # Certificate basics, common errors
│   │   └── efficiency/             # Let's Encrypt, cert-manager, renewal
│   ├── kubernetes_networking/      # K8s networking
│   │   ├── efficiency/             # Services, Ingress, Network Policies
│   │   └── mastery/                # CNI, kube-proxy, advanced patterns
│   ├── cloud_networking/           # Cloud VPCs and networking
│   │   ├── efficiency/             # VPC design, security groups, routing
│   │   └── mastery/                # VPC peering, Transit Gateway, PrivateLink
│   ├── service_mesh/               # Service mesh architecture
│   │   └── mastery/                # Istio, Linkerd, traffic management
│   ├── images/                     # Site images and diagrams
│   └── stylesheets/                # Custom CSS
├── mkdocs.yaml                     # Site configuration and navigation
├── pyproject.toml                  # Poetry dependencies
├── CLAUDE.md                       # Content guidelines and quality standards
└── README.md                       # This file
```

## Development

### Prerequisites

- Python 3.8+
- Poetry (for dependency management)

### Setup

```bash
# Install dependencies
poetry install

# Serve locally (http://localhost:8000)
poetry run mkdocs serve

# Build static site with link validation
poetry run mkdocs build --strict
```

### Writing Content

Articles follow rigorous quality standards:

**Content Standards:**
- Urgent incident scenario openings (production outage at 2am)
- Quick Start sections (solve the problem in 5 minutes)
- Mermaid diagrams for network flows and architecture
- Card grids and tabs for visual organization
- Real-world examples from platform engineering
- Practice problems with nested solutions
- Key takeaways tables
- Further reading organized by category

**Formatting Requirements:**
- Command names in backticks (`dig`, `curl`, `tcpdump`)
- Blank lines before all lists (critical for rendering)
- External links validated with WebFetch
- Code blocks with titles and line numbers
- Context before commands (never start with syntax)

**Quality Checklist:**
- NO REPETITION audit across published articles
- Pre-publication link audit (4-step process)
- Visual elements present (diagrams, cards, tabs)
- Serves multiple learning styles
- Cross-links to [cs.bradpenney.io](https://cs.bradpenney.io) for CS fundamentals

See `CLAUDE.md` for comprehensive content guidelines (~50 quality standards).

## Deployment

The site deploys automatically via GitHub Actions on push to `main`:

1. GitHub Actions builds the site using `mkdocs gh-deploy`
2. Static files are pushed to the `gh-pages` branch
3. GitHub Pages serves the content
4. Custom domain: `networking.bradpenney.io` (via CNAME in `docs/`)

## Tech Stack

- **Framework**: [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- **Theme**: Material (slate color scheme, black/amber palette)
- **Features**: Code annotations, tabs, Mermaid diagrams, copy buttons
- **Plugins**: Search, htmlproofer (link validation)
- **Deployment**: GitHub Pages with custom domain

## Contributing

This is a personal learning repository. Content improvements and corrections are welcome via issues or pull requests.

## License

Content is available for personal and educational use. See individual files for specific licensing.

## Related Sites

This site is part of an integrated learning ecosystem:

- **[cs.bradpenney.io](https://cs.bradpenney.io)** - Computer Science fundamentals (protocols, theory, packet structure)
- **[linux.bradpenney.io](https://linux.bradpenney.io)** - Linux for SREs (network commands, /etc/network configuration)
- **[k8s.bradpenney.io](https://k8s.bradpenney.io)** - Kubernetes (Services, Ingress, Network Policies integration)
- **[tools.bradpenney.io](https://tools.bradpenney.io)** - Development tools (curl, dig, jq for parsing network responses)
- **[storage.bradpenney.io](https://storage.bradpenney.io)** - Storage for SREs (network-attached storage, NFS, iSCSI)
- **[python.bradpenney.io](https://python.bradpenney.io)** - Python programming (network automation scripts)
- **[bradpenney.io](https://bradpenney.io)** - Main hub

**How they connect:**
- Linux site + Networking site = Complete system administration
- Kubernetes site + Networking site = Full K8s networking mastery
- Tools site + Networking site = Efficient network debugging
- CS site provides theory behind TCP/IP, DNS, routing protocols

---

Built with Material for MkDocs | Deployed on GitHub Pages
