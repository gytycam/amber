# MeizhiLian Resource Navigator

MeizhiLian Resource Navigator is a specialized technical resource aggregation and external link management system designed for developers, researchers, and technical teams who need to organize, categorize, and rapidly access distributed online resources. Unlike traditional bookmark managers or simple link directories, this project provides a structured approach to resource governance, offering version-aware link tracking, availability monitoring, and context-rich documentation for each external reference.

The project targets technical leads, documentation engineers, and infrastructure teams who manage large volumes of external references across multiple projects. It solves the fundamental problem of link rot, context drift, and discoverability by providing a centralized, version-controlled manifest of external resources with operational metadata. Each resource entry is annotated with its purpose, expected availability windows, and relationships to internal systems, enabling teams to proactively manage their external dependencies.

## 功能概览

- **Resource Manifest Management** - Maintains a versioned inventory of external resource URLs with status flags, update timestamps, and change history tracking for audit compliance.

- **Availability Health Monitoring** - Periodically probes each registered URL to detect downtime, certificate expiry, or content changes, generating alerts when resources deviate from expected states.

- **Category-Based Organization** - Supports multi-dimensional classification of resources by domain, purpose, geographic relevance, and internal project association.

- **Markdown-Driven Documentation Integration** - Embeds resource references directly into technical documentation pipelines, ensuring that external links remain consistent across all project materials.

- **Search and Filter Interface** - Provides a lightweight web interface for querying resources by keyword, status, category, or last-updated date, with results rendered in plain HTML.

- **Batch Import and Export** - Supports CSV and JSON exchange formats for migrating resource lists between environments or integrating with external asset management platforms.

- **Audit Logging** - Records every modification to the resource manifest, including who made the change, when, and why, supporting compliance with internal change management policies.

## 应用场景

1. **Technical Documentation Maintenance** - Documentation teams can embed resource IDs from the navigator into their markdown files, ensuring that every external reference is tracked and can be automatically verified during the build process. This reduces broken links in published documentation.

2. **Regional Service Dependency Mapping** - For organizations operating in multiple regions, this navigator helps catalog region-specific endpoints (e.g., api-cn.example.com vs api-us.example.com) with annotations about expected latency, data residency, and fallback behavior.

3. **Open Source Project Resource Indexing** - Open source maintainers can use this system to curate a verified list of upstream dependencies, reference implementations, and community resources, making it easier for new contributors to find relevant external materials.

4. **Incident Response Playbook Integration** - Operations teams can reference resource IDs within runbooks; during an incident, the navigator provides quick access to dashboards, logs, and vendor status pages, with pre-checked availability status.

5. **Onboarding Knowledge Base** - New team members can use the navigator as a curated starting point to discover internal tools, external APIs, and learning materials, with each resource annotated by its relevance to different roles.

## 快速开始

Prerequisites: Git, Node.js (v16 or later), npm.

```bash
# Clone the repository
git clone https://github.com/meizhilian/navigator.git
cd navigator

# Install dependencies
npm install

# Copy example environment configuration
cp .env.example .env

# Initialize the resource database with default manifests
npm run init-db

# Start the development server
npm run dev
```

The application will start on port 3000 by default. Access the web interface at `http://localhost:3000`. The resource manifest is stored in `data/manifest.json` and can be edited manually or through the API endpoints.

## 安装要求

| Dependency | Required Version | Description |
|------------|------------------|-------------|
| Node.js | 16.x or higher | Runtime environment for the application server |
| npm | 8.x or higher | Package manager for installing dependencies |
| SQLite3 | 3.35+ (bundled) | Embedded database for resource metadata storage |
| Git | 2.30+ | Version control for cloning and updating the repository |
| curl / wget | Any recent version | Used by the health checker for URL probing |
| openssl | 1.1.1+ | Certificate verification for HTTPS resource checks |
| cron / systemd timer | Optional | For scheduling periodic health checks |
| python3 | 3.8+ (optional) | Required for advanced data analysis scripts |

## 文档导航

| Aspect | Directory | Questions Addressed |
|--------|-----------|---------------------|
| User Guide | `docs/guide/` | How to add, remove, or categorize resources; how to interpret health status indicators; how to use the search interface. |
| API Reference | `docs/api/` | What endpoints are available for programmatic resource management; request and response schemas; authentication requirements. |
| Operations Manual | `docs/ops/` | How to deploy the navigator in production; how to configure monitoring; how to perform database backups and recovery. |
| Contributing Guide | `docs/contributing/` | How to submit new resource entries, propose categories, or improve the codebase; code style and commit message conventions. |
| Architecture Design | `docs/architecture/` | Why the system uses SQLite instead of a full RDBMS; how the health checker schedules probes; data flow diagrams for resource updates. |
| Security Policy | `docs/security/` | How credentials are stored; rate limiting on API endpoints; handling of sensitive internal URLs that should not be publicly listed. |

## 资源列表

This project curates and indexes the following external resources. All URLs are reproduced exactly as provided, without any normalization or modification.

### Domain Resources (Bare Domains)

- <code>meizhilianzhugongbang.asia</code>
- <code>meizhilianzhibo.asia</code>
- <code>meizhiliantuijian.asia</code>
- <code>meizhiliansheshoubang.asia</code>
- <code>meizhiliansaicheng.asia</code>
- <code>meizhilianqianzhan.asia</code>
- <code>meizhilianjishibifen.asia</code>

These domains represent the primary external resource endpoints tracked by this navigator. Each domain is monitored for availability and is associated with specific categories within the internal classification system. The bare-domain format is intentional, as these resources are accessed via multiple subpaths and protocols depending on the use case. The navigator stores the exact user-provided string to preserve the original reference, avoiding any interpretation or correction that might mask underlying issues with DNS configuration or certificate validity.

## 项目结构

```
navigator/
├── src/
│   ├── core/
│   │   ├── manifest.js          # Resource manifest CRUD operations
│   │   ├── health-checker.js    # URL probing and status evaluation
│   │   └── category-engine.js   # Classification rule processing
│   ├── web/
│   │   ├── server.js            # Express application entry point
│   │   ├── routes/              # API and view route definitions
│   │   └── templates/           # Plain HTML templates (no frameworks)
│   ├── cli/
│   │   ├── import.js            # Batch import from CSV/JSON
│   │   ├── export.js            # Manifest export utilities
│   │   └── validate.js          # URL format and reachability validator
│   └── lib/
│       ├── db.js                # SQLite connection and query helpers
│       ├── logger.js            # Structured logging (JSON format)
│       └── config.js            # Environment variable parsing and defaults
├── data/
│   ├── manifest.json            # Primary resource list (hand-editable)
│   ├── history/                 # Change log snapshots (one per commit)
│   └── cache/                   # Health check result cache (TTL 1 hour)
├── tests/
│   ├── unit/                    # Component-level tests (mocha)
│   ├── integration/             # API endpoint tests (supertest)
│   └── fixtures/                # Mock resource lists for testing
├── docs/                        # Full documentation (see navigation table)
├── scripts/
│   ├── init-db.js               # Database schema creation
│   ├── schedule-checks.sh       # Cron wrapper for health monitoring
│   └── migrate-v1-to-v2.js      # Manifest format migration tool
├── .env.example                  # Environment variable template
├── package.json                  # npm dependencies and scripts
├── README.md                     # This file
└── LICENSE                       # MIT license text
```

## 贡献指南

1. **Fork and Clone** - Fork the repository on GitHub and clone your fork locally. Create a new branch with a descriptive name related to your contribution (e.g., `add-resource-category` or `fix-health-check-timeout`).

2. **Update the Manifest** - If you are adding or modifying resources, edit `data/manifest.json` following the existing schema. Each resource entry must include: `id` (UUID), `url` (exact user-provided string), `category`, `description`, and `status` (active/deprecated/archived). Run `npm run validate` to ensure your changes conform to the schema.

3. **Write Tests** - For any code changes, add corresponding unit or integration tests under the `tests/` directory. Ensure existing tests pass by running `npm test`. For resource-only contributions (no code), provide a brief rationale in the pull request description.

4. **Update Documentation** - If your contribution affects user-facing behavior, update the relevant documentation in `docs/guide/`. For new categories or resource types, add a short section explaining the classification rationale.

5. **Submit a Pull Request** - Push your branch to your fork and open a pull request against the main repository's `develop` branch. Include a clear title and description, referencing any related issues. The maintainers will review your submission, provide feedback, and merge it once it meets the quality standards.

## 常见问题

**Q: Why are the URLs stored as bare domains without protocol prefixes? Does this cause issues with the health checker?**

A: The URLs are stored exactly as provided by the user to preserve the original reference. The health checker automatically attempts both HTTP and HTTPS probes for bare domains, following redirects where appropriate. This approach allows the navigator to detect protocol-specific availability issues (e.g., HTTPS misconfiguration) that would be masked if we normalized all URLs to `https://`. The web interface displays the original string but probes using a configurable protocol preference order defined in `config.js`.

**Q: How frequently are health checks performed, and can I adjust the interval?**

A: The default health check interval is 60 minutes for all registered resources. This frequency can be adjusted by modifying the `CHECK_INTERVAL_MS` environment variable (value in milliseconds) or by editing the `schedule-checks.sh` script if using cron-based scheduling. For mission-critical resources, we recommend setting a shorter interval (e.g., 5 minutes) by overriding the environment variable in your deployment configuration. Note that probing too frequently may be flagged as suspicious activity by some resource providers.

**Q: Can I use this navigator with resources that require authentication (e.g., internal APIs with API keys)?**

A: Yes, the manifest schema supports an optional `auth` field where you can specify a reference to a credentials store (e.g., environment variable name or secrets manager key). The health checker will attempt to include the appropriate authentication headers if configured. However, we strongly recommend against storing actual credentials in the manifest file; use environment variables or a dedicated secrets management solution, and reference them via the `auth` field. The `docs/ops/authentication.md` guide provides detailed examples for common authentication patterns.

## 许可证

MIT License

Copyright (c) 2026 MeizhiLian Project Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:13
