# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Repository Overview

This is a Material for MkDocs site documenting practical networking for SREs and Platform Engineers—DNS, load balancers, TLS, VPCs, Kubernetes networking, and troubleshooting. The site exists to **remove networking mystery** so platform engineers can focus on their actual job: keeping services connected and debugging production issues fast.

**The Practical Approach**: Traditional networking courses start with OSI model theory and packet tracer simulations. This site starts with "The API is timing out at 2am—how do I debug this?" We teach networking through real incidents, practical troubleshooting, and production scenarios.

**Strategic Alignment**: This site integrates with the other exploring_* sites:
- **Linux Link**: Linux teaches network commands, this site teaches when/why to use them
- **Kubernetes Link**: K8s teaches Services/Ingress, this site teaches how they actually route traffic
- **Tools Link**: Tools site teaches curl/dig, this site teaches what to curl and how to interpret responses
- **Storage Link**: Network-attached storage (NFS, iSCSI) bridges networking and storage concepts

## Target Persona

**Who they are:** SRE or Platform Engineer responsible for keeping platforms stable and/or building integrations into platforms

**Background:** May or may not have traditional software development background. Could be:

- Former sysadmin learning infrastructure as code
- Developer transitioning to platform/reliability work
- DevOps engineer building CI/CD integrations
- Platform engineer building internal tools and services

**Their job:**

- Maintain platform stability (uptime, performance, reliability)
- Respond to incidents (on-call rotations, debugging under pressure)
- Build integrations (APIs, data pipelines, automation)
- Manage infrastructure as code (Terraform, Ansible, K8s manifests)
- Work primarily in terminal environments (SSH, remote servers)

**Core networking pain points:**

- "The API is returning 502 errors—is it the load balancer, DNS, or the app?" (needs troubleshooting skills)
- "Users can't access the service—is it a firewall rule blocking traffic?" (needs security group debugging)
- "The TLS certificate expired and took down production" (needs certificate management)
- "Pod can't reach the database—is it a Kubernetes Network Policy?" (needs K8s networking understanding)
- "This API call is slow—is it network latency or the application?" (needs performance debugging)

**The transformation:** From fighting their tools → commanding their tools. Remove tool friction so they can focus on their actual job: keeping platforms running and building reliable systems.

**Opening pattern:** Start with THEIR specific incident scenario → introduce the networking concept/tool as the solution

Example opening:
> "It's 2am. The API is timing out. Users are getting 502 errors. You check the application logs—nothing. The pods are running. But something between the client and your service is broken. **This is when you need to understand load balancer health checks.**"

## Site Structure

**NOT organized by learning levels** - SREs/Platform Engineers need tools by urgency and job context, not academic progression.

### Structure: Essentials → Efficiency → Mastery

**📦 Essentials** (Debug production issues today)

- DNS Debugging (dig, nslookup, why DNS always breaks)
- Network Troubleshooting (ping, traceroute, mtr, curl basics)
- Firewall Basics (security groups, iptables, blocked traffic)
- Load Balancers (health checks, 502 errors, sticky sessions)

**⚡ Efficiency** (Design and deploy confidently)

- TLS/SSL Certificates (Let's Encrypt, cert-manager, renewal automation)
- VPC Design (CIDR blocks, routing tables, subnet strategy)
- Kubernetes Networking (Services, Ingress, Network Policies)
- CDN Basics (CloudFlare, CloudFront, caching strategies)

**🎯 Mastery** (Advanced architecture and performance)

- Service Mesh (Istio/Linkerd for traffic management)
- Advanced Troubleshooting (tcpdump, Wireshark, eBPF tools)
- Network Performance (latency optimization, bandwidth tuning)
- Multi-Region Networking (cross-region traffic, disaster recovery)

**Why this works:**

- **No arbitrary levels** - pick what you need when you need it
- **Clear value prop** - "Essentials" means you need this NOW
- **Respects their time** - Mastery is optional, not required
- **Job-focused** - organized by "what helps me do my job"

## Content Inventory by Category

This is the planned content aligned with the Essentials → Efficiency → Mastery structure:

### 📦 Essentials

**DNS Debugging:**
- dig and nslookup commands for DNS queries
- Understanding /etc/resolv.conf and /etc/hosts
- Common DNS failures (NXDOMAIN, SERVFAIL, timeouts)
- DNS propagation and TTL
- Debugging "it works on my machine but not production"

**Network Troubleshooting:**
- ping for basic connectivity testing
- traceroute/mtr for path analysis
- curl for HTTP/HTTPS debugging
- netcat (nc) for port testing
- Understanding timeouts vs connection refused

**Firewall Basics:**
- Security groups in AWS/GCP/Azure
- iptables fundamentals (INPUT, OUTPUT, FORWARD)
- Common firewall rule patterns
- Debugging blocked traffic
- Understanding stateful vs stateless firewalls

**Load Balancers:**
- Health check configuration and failures
- Understanding 502/503/504 errors
- Sticky sessions and session affinity
- Load balancing algorithms (round-robin, least connections)
- Application vs Network Load Balancers

### ⚡ Efficiency

**TLS/SSL Certificates:**
- Certificate basics (public/private keys, CA, chains)
- Let's Encrypt and ACME protocol (protocol-level: challenges, rate limits, automation tiers)
- Certificate renewal automation
- Common TLS errors (expired, hostname mismatch; self-signed = anti-pattern)
- NOTE (2026-07-08 boundary rule): cert-manager's Kubernetes machinery lives on the k8s site
  (`efficiency/networking/cert_manager.md`) — this site stays platform-agnostic and cross-links it.

**VPC Design:**
- CIDR block planning and subnet allocation
- Public vs private subnets
- Routing tables and Internet Gateways
- NAT Gateways for private subnet egress
- Security group vs NACL

**Kubernetes Networking:**
- Services (ClusterIP, NodePort, LoadBalancer)
- Ingress controllers and routing rules
- Network Policies for pod-to-pod traffic
- DNS in Kubernetes (CoreDNS)
- Understanding kube-proxy and iptables rules

**CDN Basics:**
- How CDNs reduce latency
- Cache control headers
- CloudFlare/CloudFront setup
- SSL/TLS at CDN edge
- Purging and invalidation

### 🎯 Mastery

**Service Mesh:**
- Istio architecture (control plane, data plane)
- Linkerd as lightweight alternative
- Traffic management (canary, blue/green)
- mTLS and service identity
- Observability with service mesh

**Advanced Troubleshooting:**
- tcpdump for packet capture
- Wireshark for packet analysis
- ss and netstat for socket inspection
- eBPF tools for kernel-level tracing
- Performance analysis with bpftrace

**Network Performance:**
- Latency vs throughput optimization
- TCP tuning (window size, congestion control)
- Understanding network saturation
- Bandwidth monitoring and alerting
- Impact of network on application performance

**Multi-Region Networking:**
- VPC peering across regions
- Transit Gateway architecture
- PrivateLink for service connectivity
- Cross-region latency considerations
- Disaster recovery networking patterns

## Visual & Editorial Style

**Screenshots:**
- Heavy use of screenshots for VS Code/Terminal setup
- Before/after comparisons showing tool impact
- Annotated screenshots highlighting key UI elements
- Terminal output should be readable (good contrast, reasonable width)

**GIFs/Videos:**
- Essential for showing Vim motions (show the keystrokes + result)
- Terminal workflows benefit from animation (tmux, fzf navigation)
- Keep GIFs short (< 10 seconds) and focused on one concept
- Consider embedding asciinema recordings for terminal demos

**Tone:**
- "Let me show you a trick that will save you 100 hours this year"
- Time-saving value proposition front and center
- Acknowledge the learning curve honestly
- Urgency without stress (tools that help during incidents)

## Important Preferences

**Git Operations**: The user handles all git operations (commits, pushes, etc.) themselves. Do not commit or push changes.

**MkDocs Operations**: The user handles running `mkdocs serve` and `mkdocs build` themselves. Do not run these commands.

## SEO Strategy and Publication Process

**CRITICAL**: This site uses a draft/publish workflow to ensure only vetted content appears in search engines and the sitemap.

### SEO Configuration Overview

The site has comprehensive SEO optimization:

1. **Sitemap**: Auto-generated by MkDocs at `/sitemap.xml` when `site_url` is configured
2. **robots.txt**: Located at `docs/robots.txt`, points to sitemap
3. **Meta plugin**: Injects canonical URLs to prevent duplicate content
4. **Social cards**: Open Graph images auto-generated for social media sharing
5. **Google Analytics**: Configured with tracking ID
6. **Exclude plugin**: Prevents unpublished content from appearing in builds and sitemap

### Required Metadata for Every Article

**MANDATORY**: Every article MUST have frontmatter metadata before being published:

```yaml
---
date: "2026-01-15 12:00"
title: Clear, Descriptive Title (50-60 chars ideal)
description: Compelling description for search results (150-160 chars ideal)
---
```

**Rules:**

- **Date**: Required — the RSS feed dates entries from this field. Format `date: "YYYY-MM-DD HH:MM"`. Omitting it logs an RSS build warning and leaves the feed entry undated.
- **Title**: Should be unique across the site, descriptive, include key terms
- **Description**: Summarize what the reader will learn, compelling call-to-action
- **No keywords needed**: Modern search engines don't rely on keyword meta tags
- **Check length**: Titles >60 chars and descriptions >160 chars get truncated in search results

### The Exclude Plugin Strategy

**Problem**: MkDocs by default includes ALL `.md` files in builds and sitemaps, even draft/unpublished content.

**Solution**: The `mkdocs-exclude` plugin configured in `mkdocs.yaml` excludes unpublished directories from:
- Site builds
- Sitemap generation
- Search indexing
- Navigation (even if accidentally uncommented)

**Current exclude configuration** (as of 2026-02-16):

```yaml
plugins:
  - search
  - meta
  - exclude:
      glob:
        - "level_1/*"
        - "level_2/*"
        - "level_3/*"
        - "level_4/*"
        - "level_5/*"
        - "level_6/*"
  # ... other plugins
```

**What this means:**
- Draft articles can exist in these directories without appearing in search results
- Articles can be worked on incrementally without affecting SEO
- Only vetted, published content appears in sitemap and builds

### How to Publish an Article

When an article is ready for publication (passes all quality checks), follow these steps **in order**:

#### 1. Pre-Publication Checklist

Complete the [Quality Standards Checklist](#quality-standards-checklist) below. Do NOT proceed until all items are checked.

#### 2. Remove from Exclude List

Edit `mkdocs.yaml` and remove the directory from the exclude plugin:

**Before (draft):**
```yaml
- exclude:
    glob:
      - "level_1/*"  # Article is excluded
      - "level_2/*"
```

**After (published):**
```yaml
- exclude:
    glob:
      # - "level_1/*"  # REMOVED - now published
      - "level_2/*"
```

**IMPORTANT**:
- Remove the ENTIRE line, don't just comment it
- If publishing individual files (not whole directories), use specific paths:
  ```yaml
  - "level_1/overview.md"  # Still draft
  - "level_1/pods.md"      # Still draft
  # level_1/services.md is published (not in exclude list)
  ```

#### 3. Add to Navigation

Uncomment the article in the `nav:` section of `mkdocs.yaml`:

**Before:**
```yaml
# - Level 1 - Core Primitives:
#     - Overview: level_1/overview.md
#     - Pods Deep Dive: level_1/pods.md
```

**After:**
```yaml
- Level 1 - Core Primitives:
    - Overview: level_1/overview.md
    - Pods Deep Dive: level_1/pods.md
    # - Services: level_1/services.md  # Still in draft
```

#### 4. Verify Publication

Run these commands to verify:

```bash
# Build the site
poetry run mkdocs build --strict

# Check sitemap includes the new article
grep -o '<loc>[^<]*</loc>' site/sitemap.xml | grep level_1

# Verify the article appears in search index
grep -i "pods deep dive" site/search/search_index.json
```

**Expected results:**
- Sitemap includes the new article URL
- Search index contains the article
- No build errors from htmlproofer
- All internal links work

#### 5. Update CLAUDE.md Exclude List

Update the "Current exclude configuration" section above to reflect what's NOW excluded (for future reference).

### SEO Checklist for Published Articles

Before removing an article from the exclude list, verify:

- [ ] **Frontmatter metadata present** - Title and description in YAML frontmatter
- [ ] **Title is unique** - Not duplicated across other published articles
- [ ] **Description is compelling** - 150-160 chars, summarizes value
- [ ] **All images have alt text** - Check with `grep -r '!\[' article.md`
- [ ] **All links work** - Internal links point to published articles only
- [ ] **No "coming soon" dead links** - Replace with plain text or link to overview
- [ ] **External links are valid** - Use WebFetch to verify important URLs
- [ ] **Headings are hierarchical** - One H1, logical H2-H6 structure
- [ ] **No duplicate content** - Cross-link instead of repeating other articles

### Common SEO Mistakes to Avoid

1. **Linking to unpublished articles** - Always check if target article is in exclude list before linking
2. **Forgetting to update exclude list** - When publishing, remove from exclude glob
3. **Missing metadata** - Every article needs title and description
4. **Publishing incomplete articles** - Follow full quality checklist before publishing
5. **Leaving articles in navigation but excluded** - Navigation and exclude list must align

### SEO Monitoring

After publication, verify search engine indexing:

1. **Check Google Search Console** - Verify pages are indexed
2. **Monitor sitemap errors** - Search Console reports sitemap issues
3. **Verify canonical URLs** - Use browser dev tools to check `<link rel="canonical">`
4. **Test social cards** - Share on social media to verify Open Graph images

## CRITICAL: No Repetition - Respect Reader's Time

**This is an absolute deal-breaker for content quality.**

### The Principle

Avoid duplication and repetition at all costs. Every time we repeat information, we waste the reader's time and make the content feel bloated.

### The Rules

1. **Cross-link instead of repeating** - If a concept is explained elsewhere, link to it
2. **Only repeat for significantly different perspectives** - Brief intro vs. deep dive is acceptable; same explanation twice is not
3. **Progressive depth, not repetition** - Each article builds WITHOUT re-explaining previous articles
4. **Audit before publishing** - Search for repeated concepts across published articles

### Before Explaining Any Concept, Ask:

1. Have we explained this elsewhere in this section?
2. If yes, is my perspective SIGNIFICANTLY different?
3. If no, add a cross-link: "Remember X from [Article]? Now let's see how..."
4. If yes, explicitly state the new angle: "Earlier we covered X in Git basics - now let's see how NeoVim integrates with it"

### Required: Pre-Publication Repetition Audit

Before marking any article complete, use the Explore agent to search for repeated concepts across published articles in the same section. If found, consolidate and cross-link.

## Project Structure

- `docs/` - Markdown content organized by tool category
  - `git/core_functionality/` - Git fundamentals and configuration
  - `nvim/` - NeoVim guides and philosophy
  - `automation/` - Makefiles, semantic versioning (planned)
  - `images/` - Screenshots and visual assets
  - `stylesheets/` - Custom CSS (extra.css)
- `mkdocs.yaml` - Site configuration and navigation
- `pyproject.toml` - Poetry dependencies

**Important:** Directory structure mirrors site navigation. Articles reference each other using relative paths.

## Common Commands

```bash
# Install dependencies
poetry install

# Serve locally (http://localhost:8000)
poetry run mkdocs serve

# Build static site (ALWAYS use --strict for link validation)
poetry run mkdocs build --strict

# Validate all internal links
# The htmlproofer plugin is configured to:
# - Validate all internal links (raise_error: true)
# - Skip external URL validation (validate_external_urls: false)
# - Treat warnings as errors (--strict flag)
```

**Link Validation:** The project uses `mkdocs-htmlproofer-plugin` to validate all internal links. Always build with `--strict` flag to catch broken links.

## Content Guidelines

### Tone and Style

Articles must balance **urgency with clarity** and be **immediately actionable** while remaining **technically accurate**. The goal: help SREs and Platform Engineers overcome tool friction so they can focus on keeping platforms running.

**Core Principles:**

- **Strong openings**: Ground in real-world SRE/platform problems (incident response, config debugging, repetitive tasks, SSH environments)
- **Professional yet engaging**: Use wit in parentheticals and asides, not emoji spam (limit to 1-3 per article, used strategically)
- **Technical rigor**: Include precise terminology, explain underlying concepts, provide historical context where relevant
- **Structured learning**: Build from simple to complex examples; use clear section headers
- **Thoughtful closings**: Tie tools to broader software development philosophy; avoid jokey endings
- **Direct voice**: Address reader as "you"; be confident but not arrogant; educational but not preachy
- **Practical focus**: Show real-world use cases, not just abstract examples

**Required Sections:**

1. Opening paragraph(s) - hook with real-world SRE/platform problem (incident at 2am, config debugging, SSH-only environment)
2. Clear explanation of what the tool is and how it solves their problem
3. Quick Start - get productive in 5 minutes (essential commands/config)
4. Technical details (commands, configuration, workflows)
5. "Why [Tool] Matters for Platform Work" section - practical importance for SRE/platform engineering
6. Common Scenarios with solutions (incident response, routine tasks, automation)
7. Practice Problems with full solutions (use `??? question` and `??? tip`)
8. "Key Takeaways" table - summarize essential concepts
7. "Further Reading" section with links to official docs and quality resources
8. Closing paragraph(s) - thoughtful reflection on tool's philosophy or impact

**Examples of Good Tone:**
- "It's 2am. The API is timing out. You check the application—it's fine. You check the database—it's fine. The problem is somewhere in the network between them. This is when you need `traceroute`." (urgent, real-world scenario)
- "The load balancer shows all targets healthy, but users are getting 502 errors. This isn't a contradiction—it's a health check misconfiguration." (clear problem diagnosis)
- "You've been manually renewing TLS certificates and hoping you remember before they expire. Let's automate this with cert-manager." (acknowledges their pain)

**Avoid:**
- Excessive emojis (🛠️✨🎮😄 scattered everywhere)
- Over-the-top phrases like "amazing!", "incredible!", "mind-blowing!"
- Condescending language or talking down to readers
- Jokey closings that undermine the technical content
- Creating files unless absolutely necessary

### Critical Formatting Rules

#### Command Names in Prose
**ALWAYS wrap command names in backticks when mentioned in regular text.**

- ✅ Correct: "Use `jq` to parse JSON responses"
- ✅ Correct: "The `git`, `tmux`, and `fzf` commands"
- ❌ Wrong: "Use jq to parse JSON responses"
- ❌ Wrong: "The git, tmux, and fzf commands"

**Exceptions:**
- Command names inside code blocks (already formatted)
- Command names in mermaid diagrams (doesn't render backticks)
- URLs or file paths

**Common commands to watch for:**
`git`, `jq`, `yq`, `vim`, `nvim`, `tmux`, `screen`, `fzf`, `ripgrep`, `docker`, `kubectl`, `make`, `ssh`, `curl`, `wget`, `cat`, `grep`, `sed`, `awk`

This applies to:
- Prose paragraphs
- Table cells
- List items
- Admonition text
- Further reading descriptions
- Exercise instructions

#### Markdown List Formatting
**ALWAYS add a blank line before bulleted or numbered lists.**

This is a recurring issue that breaks list rendering in MkDocs.

- ❌ Wrong:
  ```markdown
  Here's what you need:
  - Item 1
  - Item 2
  ```

- ✅ Correct:
  ```markdown
  Here's what you need:

  - Item 1
  - Item 2
  ```

**When to check:**
- After paragraphs that introduce lists
- After headings that introduce lists
- Inside admonitions/callouts
- Inside card grids
- Inside collapsible sections

**Pre-publication audit:** Use Grep to find potential issues:
```bash
# Find paragraphs followed immediately by lists (no blank line)
grep -B1 "^- " file.md | grep -v "^--$" | grep -v "^- "
```

#### External Link Validation
**ALWAYS test external links before publishing an article.**

URLs can break, move, or change over time. Use WebFetch to verify each external link:

**Process:**
1. Extract all external URLs from the article (https://...)
2. Test each with WebFetch to confirm it's accessible
3. For broken links:
   - Search for updated URL with WebSearch
   - Find authoritative replacement sources
   - Update link and description if needed

**Common link issues:**
- GitHub repos archived or moved
- Official documentation reorganized
- Blog posts deleted or moved
- Tool websites restructured

**Tools:**
```bash
# Extract external links from markdown
grep -o 'https://[^)]*' file.md
```

Then test each with WebFetch tool.

### Content Structure

- Articles should be teaching-focused, not just command references
- Use mermaid diagrams for visual concepts (workflow diagrams, architecture)
- Include practice problems with expandable solutions (`??? question` / `??? tip`)
- Cross-link related articles using markdown links
- **Cross-link to cs.bradpenney.io** for computer science fundamentals
- Use admonitions for tips and callouts:
  - Prefer `??? tip` (collapsible tips) for helpful insights
  - Use `???+ warning` (expanded warnings) for important caveats like learning curves
  - **Avoid** `??? note` or `!!! note` (the "note" style doesn't render well in Material for MkDocs)

### Code Examples

**Code examples must include titles, line numbers, and annotations:**

- Format: ` ```language title="Descriptive Title" linenums="1" `
- Example: ` ```bash title="Configure Git User Identity" linenums="1" `
- The title should describe what the code demonstrates
- Material for MkDocs provides copy button automatically
- **Add code annotations** to explain key concepts:
  - Use `# (1)!`, `# (2)!`, etc. for inline annotations
  - After the code block, provide numbered explanations
  - Annotate important flags, options, or non-obvious behavior
  - Example:

    ```bash
    git config --global user.name "Jane Developer"  # (1)!
    git config --global user.email "jane@example.com"  # (2)!
    ```

    1. Your name appears in commit metadata
    2. Your email links commits to your GitHub/GitLab account

### Interactive Elements: Tabs

Use tabs for comparing tools, showing alternatives, or presenting domain-specific examples:

```markdown
=== ":material-icon-name: Tab Title"

    Content here with 4-space indentation.

    **All content** must be indented to stay within the tab.

    ```bash title="Code Example" linenums="1"
    # Code must also be indented
    command --flag
    ```

=== ":material-icon-name: Another Tab"

    Second tab content, also indented.
```

**Critical tab formatting rules:**
- Tab declarations (`===`) start at column 0
- ALL content inside tabs must be indented with 4 spaces
- Code blocks need indentation: ` ```language` line AND all code content
- Blank line between tabs (but not required)
- Use Material icons for visual appeal: `:material-rocket-launch:`, `:material-timer-outline:`, etc.

**Examples from the site:**
- NeoVim distributions (LazyVim, Kickstart, AstroNvim) - comparing alternatives

### Article Layout and Visual Structure

**CRITICAL**: Articles must not be simple command lists. They must teach skills with context and serve multiple audiences through varied layout.

#### Visual Elements Required

1. **Mermaid Diagrams** (when workflow exists)
   - Place at article top to show the big picture
   - Use for: workflows, decision trees, tool relationships, architecture
   - Keep them simple and focused on one concept
   - Use TD (top-down) vertical flow for readability
   - Example: Git workflow from modified → staged → committed

2. **Card Grids** (for categorizing related concepts)
   - Use `<div class="grid cards" markdown>` for grouping related concepts
   - Each card should explain "Why it matters" BEFORE showing commands
   - Ideal for: 3-4 related tools/workflows that are peers (not sequential)
   - Format:
     ```markdown
     <div class="grid cards" markdown>

     -   :material-icon: **Card Title**

         ---

         **Why it matters:** Context and relevance for SRE/platform work

         ``` bash title="Command" linenums="1"
         command
         ```

         **Key insight:** What to look for or understand

     </div>
     ```

3. **Content Tabs** (for comparing alternatives or platforms)
   - Use `=== "Tab Name"` for alternative approaches or tools
   - Perfect for: tool comparisons, different workflows, platform-specific instructions
   - Examples:
     - Tool tabs: "jq" vs "Python json module" vs "yq"
     - Editor tabs: "Vim" vs "NeoVim" vs "VS Code"
     - Platform tabs: "macOS" vs "Linux" vs "Windows"

#### Layout Patterns by Article Type

**Tool Introduction Articles (Git, JQ, YQ, tmux, etc.):**

Structure should be:

1. **Opening** - Urgent SRE/platform problem hook (incident scenario, tool friction pain)
2. **Quick Start** - Get productive in 5 minutes (essential commands)
3. **Mermaid Diagram** - Show the workflow or tool architecture
4. **Card Grid** - Group related concepts with "why it matters" context
5. **Scenario Tabs** - Different use cases (incident response, daily work, automation)
6. **Common Patterns** - Real-world examples from platform work
7. **Troubleshooting** - Common issues and solutions
8. **Practice Exercises** - Hands-on reinforcement
9. **Key Takeaways** - Table summarizing essential concepts
10. **What's Next** - Progression to related tools
11. **Further Reading** - Organized by category

**Workflow Articles (Git workflows, tmux sessions, shell productivity):**

Structure should be:

1. **Opening** - Why you need this workflow (pain point)
2. **Prerequisites** - What tools you need first
3. **Workflow Tabs** - Different approaches or scenarios
4. **Step-by-step with explanations** - The "how" with the "why"
5. **Real-world Example** - Complete walkthrough with actual commands
6. **Verification** - How to check it worked
7. **Troubleshooting** - Common issues (collapsible)
8. **Practice Exercises**
9. **Key Takeaways**
10. **Further Reading**

**Comparison Articles (Editor choices, shell options, automation tools):**

Structure should be:

1. **Opening** - Why this choice matters for SREs/platform engineers
2. **Comparison Table** - Quick decision matrix
3. **Tool Tabs** - Deep dive into each option
4. **Use Case Scenarios** - Which tool for which situation
5. **Migration Guide** - Moving between tools if needed
6. **Practice Exercises**
7. **Key Takeaways**
8. **Further Reading**

#### Context Before Commands

**NEVER start with command syntax.** Always provide context first:

- ❌ Bad: "Run `jq '.items[]'` to extract array elements"
- ✅ Good: "You need to extract pod names from kubectl JSON output. The `.items[]` filter in `jq` lets you..."

**In cards: "Why it matters" must come before the command**
**In scenarios: Goal and context must come before commands**
**In sections: Real-world relevance must precede technical details**

#### Serve Multiple Audiences

Every substantial article should provide:

1. **Visual learners** → Mermaid diagrams, card layouts, screenshots
2. **Hands-on learners** → Quick start sections, practice exercises, scenario tabs
3. **Reference seekers** → Quick reference tables/checklists, command summaries
4. **Deep divers** → Comprehensive explanations, links to official docs
5. **Speed demons** → TL;DR sections, quick recap tables

#### Further Reading Organization

**REQUIRED**: Organize Further Reading into categories:

```markdown
## Further Reading

### Official Documentation
- [Tool Official Docs](url) - Comprehensive reference
- [Tool GitHub](url) - Source code and issues

### Related Tools & Alternatives
- [Alternative Tool](url) - When to use instead
- [Complementary Tool](url) - Works well with this tool

### Deep Dives
- [Advanced Article](url) - What you'll learn
- [Performance Tuning](url) - Optimization strategies

### Community Resources
- [Tutorial Series](url) - Step-by-step guides
- [Blog Post](url) - Real-world experiences
```

**Include when relevant:**
- Official tool documentation
- GitHub repositories
- Tool creator's blog posts
- Community tutorials and guides
- Related cs.bradpenney.io articles

### Cross-Linking Strategy

**IMPORTANT**: Always cross-link to cs.bradpenney.io for computer science fundamentals.

**When to Link to CS Site:**
- Computational thinking concepts (problem decomposition, abstraction, pattern recognition)
- Data structures (DAG → Binary Trees, graphs)
- Algorithms and theory (parsing → How Parsers Work, FSMs → Finite State Machines)
- Computer science concepts underlying tools (regex → Regular Expressions)

**Example Cross-Links Added:**
- Git's DAG → `https://cs.bradpenney.io/building_blocks/binary_trees_and_representation/`
- Modal editing composability → `https://cs.bradpenney.io/building_blocks/computational_thinking/`
- Tree-sitter parsing → `https://cs.bradpenney.io/building_blocks/how_parsers_work/`
- Regex in aliases → `https://cs.bradpenney.io/building_blocks/regular_expressions/`

**Internal Links:**
- Link related tool concepts using relative paths
- Git articles link to each other for workflows and configuration
- NeoVim articles reference Git for version control context

### Pre-Publication Link Audit

**REQUIRED** before marking an article as complete:

#### Step 1: Check for Broken Links

```bash
# Extract all internal markdown links
grep -o '\[.*\](.*\.md)' article.md
```

Verify each linked article is:
- Published (uncommented in mkdocs.yaml)
- Located at the correct path
- Accessible from the current article's location

#### Step 2: Add Cross-Links Between Published Articles

Every article should link forward and backward in its logical progression:

**For tool introduction articles:**
- Link to prerequisite tools ("Before using jq, make sure you understand JSON basics from...")
- Link to related tools ("After mastering jq, you might want to learn yq for YAML")
- Link to workflow articles that use this tool

**For workflow articles:**
- Link back to tool introduction articles
- Link to related workflows
- Link to troubleshooting guides

**Example cross-link patterns:**
- "Remember from [Git Basics](../git/core_functionality/intro_to_git.md), commits are snapshots..."
- "We covered tmux session management in [Terminal Multiplexing](../efficiency/tmux.md). Now let's integrate it with..."
- "This builds on the concepts from [JQ Basics](../essentials/jq_basics.md) but adds..."

#### Step 3: Add External Authoritative Links

**Official Documentation:**
- Link to tool's official documentation site
- Link to GitHub repository (if open source)
- Link to official tutorials or guides

**Related Resources:**
- Link to cs.bradpenney.io for CS fundamentals (DAG, parsing, regex, etc.)
- Link to complementary tools' documentation
- Link to relevant RFCs or specifications (JSON RFC, YAML spec, etc.)

**Deep Dives:**
- Link to tool creator's blog posts
- Link to detailed technical articles
- Link to performance analysis or benchmarks

#### Step 4: Verify Article Series Callouts

If the article is part of a logical series (Essentials, Efficiency, Mastery), include a callout:

```markdown
!!! tip "Part of Essentials"
    This article is part of the [Essentials](../index.md#essentials) series - the core tools every SRE needs.
```

Format:
- Use `!!! tip` admonition
- Title format: "Part of [Category Name]"
- Link to category overview or index

### Practice Problems

Every article should include 2-4 practice problems:

- Use `??? question "Practice Problem N: Descriptive Title"`
- Provide solutions in nested `??? tip "Answer"` blocks
- Problems should test understanding, not just memorization
- Build from basic recall to application to synthesis
- Include explanation in answers, not just commands or facts

Example:
```markdown
??? question "Practice Problem 1: Git States"

    If you modify a file but don't run `git add`, which state is the file in?

    ??? tip "Answer"

        The file is in the **modified** state. It exists in your working directory with changes, but those changes haven't been staged for commit yet. Running `git status` would show it under "Changes not staged for commit."
```

### Key Takeaways Format

End each article with a table summarizing core concepts:

```markdown
## Key Takeaways

| Concept | What It Means |
|:--------|:--------------|
| **DVCS** | Every repository has complete history; no single point of failure |
| **Three States** | Modified → Staged → Committed workflow enables precise control |
| **Branches** | Lightweight pointers to commits; cheap to create and merge |
```

### Topic-Specific Guidelines

**DNS Articles:**
- Start with "DNS is broken" scenario
- Explain propagation and caching (TTL)
- Show dig/nslookup with real output
- Cover /etc/resolv.conf and /etc/hosts
- Link to RFC specifications where relevant

**Troubleshooting Articles:**
- Present symptoms first (timeouts, 502s, connection refused)
- Diagnostic flowchart with mermaid
- Show command output and how to interpret it
- Multiple troubleshooting paths (quick check → deep dive)
- Real packet captures when relevant

**Load Balancer Articles:**
- Explain health checks with real examples
- Show configuration for major platforms (ALB, NLB, GCP LB)
- Diagram traffic flow from client to backend
- Common misconfigurations and how to detect them
- Performance implications of different algorithms

**Kubernetes Networking Articles:**
- Assume reader knows basic K8s (Pods, Deployments)
- Show YAML configs with networking sections highlighted
- Explain how CNI plugins work
- Diagram packet flow through K8s networking stack
- Cross-link to K8s site for fundamentals

**Cloud Networking Articles:**
- Balance AWS/GCP/Azure examples
- Focus on concepts, not cloud-specific console clicks
- Show Infrastructure as Code (Terraform) where possible
- Emphasize cost implications of networking decisions
- Real architecture diagrams for VPC designs

## Working with This Repository

- Read existing content before modifying to match established tone
- Preview changes locally with `mkdocs serve` before committing
- Keep navigation in `mkdocs.yaml` organized and logical
- Ensure all internal links work (Material for MkDocs htmlproofer will validate)
- Cross-link to cs.bradpenney.io for CS fundamentals
- Use code annotations extensively
- Follow the practice problem → key takeaways → further reading structure

## Configuration Features

The site uses Material for MkDocs with these key features:

- **Theme**: Slate scheme with black/amber palette
- **Code Features**: Annotations, copy buttons, syntax highlighting
- **Content Features**: Tabs, admonitions, Mermaid diagrams
- **Validation**: htmlproofer plugin catches broken internal links
- **Extensions**: emoji, tabbed content, superfences for code/diagrams

Always build with `--strict` to catch configuration issues and broken links.

## Quality Standards Checklist

Before uncommenting an article in `mkdocs.yaml`:

**✅ Content Quality:**

- [ ] **NO REPETITION AUDIT** - Searched for repeated concepts across published articles (CRITICAL!)
- [ ] Opening hooks with real-world SRE/platform problem (incident at 2am, tool friction, SSH environment)
- [ ] Clear learning objectives stated upfront
- [ ] Quick Start section - get productive in 5 minutes with essential commands
- [ ] Tool installation instructions for major platforms (macOS, Linux, Windows)
- [ ] Real-world use cases and examples from platform engineering
- [ ] Common workflows demonstrated (incident response, daily operations, automation)
- [ ] "Why [Tool] Matters for Platform Work" section present
- [ ] Tips and best practices for SRE workflows
- [ ] Common pitfalls and troubleshooting section
- [ ] Practice exercises with nested solutions (`??? tip "Solution"` inside question)
- [ ] Key takeaways table or quick recap
- [ ] What's Next progression to related tools
- [ ] Further Reading organized into categories (Official Documentation, Related Tools, Deep Dives, Community Resources)

**✅ Technical Accuracy:**

- [ ] Commands tested and verified to work
- [ ] Output examples match what users will see
- [ ] Version information provided (which versions of tool covered)
- [ ] Installation verified for all platforms mentioned
- [ ] Cross-checked against official documentation
- [ ] Alternative tools mentioned where relevant

**✅ Tone and Style:**

- [ ] Matches urgent-yet-clear tone for SRE/platform engineers
- [ ] Time-saving value proposition is clear
- [ ] Addresses tool friction pain points
- [ ] Professional language (not overly playful or marketing-speak)
- [ ] Command names in backticks throughout (CRITICAL!)
- [ ] Emoji usage limited (1-3 max, strategic placement)

**✅ Layout and Structure:**

- [ ] Article is NOT a simple command list—teaches skills with context
- [ ] Uses visual elements appropriately (mermaid diagrams, card grids, tabs)
- [ ] Mermaid diagram at top if workflow/architecture exists
- [ ] Card grids explain "Why it matters" before commands
- [ ] Scenario tabs for different use cases when applicable
- [ ] Quick reference table/checklist for experienced users
- [ ] Serves multiple learning styles (visual, hands-on, reference, deep-dive, speed)
- [ ] Context provided BEFORE commands (never starts with syntax)
- [ ] Layout matches article type pattern (tool intro, workflow, comparison)

**✅ Formatting:**

- [ ] All code blocks have `title=` and `linenums="1"` attributes
- [ ] **CRITICAL: Blank lines before ALL lists** (recurring issue - check every list in article)
- [ ] Command names wrapped in backticks in prose (not in code blocks)
- [ ] Command examples show actual output with explanations
- [ ] Platform-specific tabs for installation/usage when needed
- [ ] Admonitions used appropriately (`??? tip`, `???+ warning`)
- [ ] Code annotations for complex commands (`# (1)!`, `# (2)!`, etc.)
- [ ] Internal links use relative paths
- [ ] **External links validated with WebFetch before publishing** (URLs break over time)

**✅ Integration and Links:**

- [ ] **Pre-publication link audit completed (4-step process)**
- [ ] **NEVER link to unpublished articles** - only link to articles uncommented in mkdocs.yaml
- [ ] Cross-links added between all published articles in logical progression
- [ ] Links to cs.bradpenney.io for CS fundamentals where relevant
- [ ] "Part of [Category]" callout with link to overview (if part of series)
- [ ] Referenced in "What's Next" from prerequisite articles
- [ ] Links to related tool articles
- [ ] Links to official documentation included
- [ ] Navigation uncommented in `mkdocs.yaml`
- [ ] No broken links (validate with `--strict` build)
