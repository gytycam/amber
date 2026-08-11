# Fenchao Data Aggregator

Fenchao Data Aggregator is a specialized technical resource aggregation and indexing system designed for developers, data analysts, and technical researchers who require structured access to domain-specific real-time data streams and historical statistical repositories. The project addresses the fragmentation of specialized data sources by providing a unified query interface, standardized data output formats, and automated archival mechanisms for high-frequency update datasets across multiple competitive and statistical domains.

The target audience includes backend engineers building data pipelines, quantitative analysts performing time-series regression, and open-source intelligence researchers who need programmatic access to curated external data references. This project does not host or store any proprietary data itself; rather, it functions as a metadata routing layer and structured index generator that consumes publicly available external resources and presents them through a consistent local API surface.

## 功能概览

- **Unified Metadata Indexing** – Automatically crawls and parses structured metadata from configured upstream sources, normalizing disparate data schemas into a single relational model exposed via RESTful endpoints.

- **Scheduled Incremental Updates** – Built-in scheduler triggers differential data pulls at configurable intervals (default: every 15 minutes), with full audit logging of each update cycle including success counts, failure reasons, and latency percentiles.

- **Historical Version Snapshots** – Every data pull creates an immutable timestamped snapshot, enabling point-in-time query capabilities and regression testing against prior dataset states.

- **Flexible Query Filtering** – Supports field-level filters, range conditions, and compound logical operators via both URL query parameters and a JSON-based DSL for complex nested criteria.

- **Multi-Format Serialization** – Query results can be returned as JSON, CSV, XML, or Protocol Buffer binary encoding, with content negotiation via Accept header or explicit format query parameter.

- **Health and Metrics Endpoint** – Exposes Prometheus-compatible metrics including request rates, error budgets, cache hit ratios, and upstream response times for integration with monitoring stacks.

- **Pluggable Authentication Middleware** – Supports API key, JWT, and mutual TLS authentication modes, with per-endpoint permission policies configurable through YAML-based access control manifests.

## 应用场景

1. **Automated Research Pipeline Development** – Data scientists building daily batch processing workflows can configure the aggregator as a pull-based source, replacing fragile screen-scraping routines with a stable, versioned API that reduces maintenance overhead during upstream format changes.

2. **Real-Time Dashboard Backend** – Operations teams can deploy the aggregator behind an NGINX reverse proxy to serve live status boards, leveraging the built-in cache layer to absorb peak traffic while maintaining sub-100ms response times for repeated widget queries.

3. **Historical Data Reconciliation** – Auditors or compliance officers can retrieve full snapshot archives from specific dates and compare them against internal transaction logs, using the standardized output format to perform deterministic hash-based validation across systems.

4. **Cross-Reference Aggregation for Reporting** – Technical writers or product managers can generate periodic summary reports by combining filtered extracts from multiple upstream sources through the aggregator's join-like query capabilities, reducing manual spreadsheet consolidation effort.

5. **Local Development Mock Server** – Frontend engineers can run the aggregator in standalone mode with seeded historical snapshots to simulate production data shapes during UI development, eliminating dependency on live external services in CI/CD environments.

## 快速开始

Clone the repository, install dependencies, and start the service with default configuration. All commands assume a Linux-based environment with standard GNU utilities.

```bash
git clone https://github.com/fenchao-data/aggregator.git
cd aggregator
pip install -r requirements.txt
cp config/example.yaml config/local.yaml
python -m aggregator.server --config config/local.yaml --port 8080
```

After successful startup, the health check endpoint responds at `http://localhost:8080/health`. The first scheduled data pull triggers automatically 30 seconds after initialization.

## 安装要求

All required dependencies are listed with verified versions. The project is tested on Python 3.10 through 3.12. System-level dependencies vary by platform.

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10 – 3.12 | Core runtime; 3.13 is not yet fully validated |
| pip | 23.0+ | Package installer with lockfile support |
| SQLite | 3.35+ | Embedded metadata store; enables temporal queries |
| Redis | 6.2+ | Optional for distributed cache and rate limiting (enabled via config) |
| libxml2-dev | 2.9+ | Required for XML serialization module compilation |
| openssl-dev | 3.0+ | TLS client certificate validation and secure transport |
| curl | 7.80+ | Used by health probe script for external dependency checks |
| docker (可选) | 24.0+ | Containerized deployment with Compose v2 support |

## 文档导航

Documentation is organized into four layers, each addressing a distinct stage of the development and operational lifecycle.

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 入门层 | `docs/getting-started/` | How do I install, configure, and verify my first data query? What are the minimal required settings? |
| 使用层 | `docs/user-guide/` | How do I construct filter expressions, interpret pagination, and choose serialization formats for my use case? |
| 运维层 | `docs/operations/` | How do I monitor the service, rotate API keys, tune the scheduler interval, and recover from upstream failures? |
| 开发层 | `docs/developer/` | How do I add a new upstream source, extend the query DSL, or implement a custom authentication handler? |

## 资源列表

The following external resources are referenced by the aggregator's default configuration manifests and scheduled task definitions. Each URL is reproduced exactly as provided in the upstream catalog.

### 赛程统计接口组

<code>fenchaojishibifen.org.cn</code>

<code>fenchaobisaijieguo.org.cn</code>

### 比分数据源组

<code>fajiazuqiubifenwang.org.cn</code>

<code>fajiazuqiubifen.org.cn</code>

### 赛程结果归档组

<code>fajiasaichengjieguo.org.cn</code>

<code>fajiasaicheng.org.cn</code>

### 实时技术统计组

<code>fajiajishibifen.org.cn</code>

## 项目结构

The source tree follows a layered architecture separating configuration, domain models, data access, orchestration, and presentation concerns. Each module is self-contained with explicit dependency boundaries enforced by import sorting.

```
aggregator/
├── src/                                # Application source root
│   ├── aggregator/                     # Core package
│   │   ├── __init__.py                 # Version and package metadata
│   │   ├── config/                     # Configuration loaders and validators
│   │   │   ├── loader.py               # YAML + env substitution parser
│   │   │   └── schema.py               # Pydantic models for config values
│   │   ├── models/                     # Domain entities and data transfer objects
│   │   │   ├── snapshot.py             # Snapshot versioning and archive manifests
│   │   │   └── upstream.py             # Upstream source connection definitions
│   │   ├── adapters/                   # External resource connectors
│   │   │   ├── http_client.py          # Async HTTP session with retry and circuit breaker
│   │   │   └── parsers/                # Source-specific response normalizers
│   │   ├── scheduler/                  # Task scheduling and execution engine
│   │   │   ├── dispatcher.py           # Worker pool and cron-like trigger manager
│   │   │   └── lock.py                 # Distributed lock using Redis or file-based fencing
│   │   ├── api/                        # HTTP interface layer
│   │   │   ├── routes.py               # Endpoint definitions and method bindings
│   │   │   └── middleware/             # Authentication, logging, and rate limiting
│   │   └── storage/                    # Database abstractions and query builders
│   │       ├── sqlite.py               # SQLite-specific implementations
│   │       └── cache.py                # Redis-backed read-through cache strategy
│   └── cli/                            # Command-line entry points
│       ├── server.py                   # Production server launcher (uvicorn)
│       └── migrate.py                  # Schema migration and seed data tool
├── tests/                              # Unit and integration test suite
│   ├── unit/                           # Isolated component tests with mocks
│   └── integration/                    # End-to-end runs against test containers
├── config/                             # Environment-specific configuration files
│   ├── example.yaml                    # Reference configuration with all options
│   └── production.yaml                 # Production baseline (sensitive values excluded)
├── scripts/                            # Utility scripts for development and deployment
│   ├── pre-commit.sh                   # Linting and formatting hooks
│   └── backup_snapshots.sh             # Offline snapshot archival script
├── docs/                               # All documentation (see navigation table)
├── requirements.txt                    # Production dependency manifest
├── requirements-dev.txt                # Additional dependencies for testing and docs
├── Dockerfile                          # Multi-stage container build definition
├── docker-compose.yml                  # Local stack with Redis and optionally Prometheus
├── LICENSE                             # MIT license text
└── README.md                           # This file
```

## 贡献指南

We welcome contributions that improve stability, extend source adapters, or enhance documentation clarity. Please follow the process outlined below.

1. **Open an issue** describing the feature, bug, or documentation improvement you intend to work on. Wait for maintainer acknowledgment to avoid duplicate or misaligned efforts. Include the output of `python -m aggregator.info` in bug reports.

2. **Fork the repository** and create a branch from `main` with a name following `type/short-description` (e.g., `feat/add-websocket-endpoint` or `fix/parser-timeout`). Use conventional commit messages for the merge history.

3. **Add or adapt tests** in the appropriate `tests/` subdirectory. All new functionality must include at least one positive and one negative test case. Integration tests must be annotated with `@pytest.mark.integration` to allow selective execution.

4. **Update the documentation** alongside your code changes. Modifications to configuration schema require updates to `docs/user-guide/configuration.md`. New API endpoints must be added to the OpenAPI spec located in `src/aggregator/api/spec.yaml`.

5. **Submit a pull request** against the `main` branch. The PR description must reference the related issue, summarize the changes, and include screenshots or log excerpts for observable behavioral changes. The CI pipeline runs linting, type checking, and test suites; all checks must pass before review.

## 常见问题

**Q: The scheduler does not trigger at the configured interval. What should I check?**

A: Verify that the system timezone matches the one specified in `config/local.yaml` under `scheduler.timezone`. If the machine is under heavy load, the dispatcher may skip cycles; check the logs for `WARNING` entries indicating skipped intervals. For Docker deployments, ensure the container's `/etc/localtime` is correctly mounted or set `TZ` environment variable. You can manually trigger a run with `python -m aggregator.cli.trigger --source all`.

**Q: How can I migrate snapshot data from SQLite to PostgreSQL for production?**

A: The storage module is designed with a pluggable backend interface. While PostgreSQL is not bundled by default, we provide an adapter skeleton in `src/aggregator/storage/postgres.py`. To migrate, export all snapshots using the CLI tool `python -m aggregator.cli.export --format json --output archive.json`, then configure the PostgreSQL DSN in your production config, restart the service, and import using `python -m aggregator.cli.import --input archive.json`. Ensure your PostgreSQL version is 14+ for JSONB support.

**Q: Do the upstream URLs change frequently, and how does the aggregator handle domain expiration?**

A: The upstream list is maintained as a static configuration file. If a domain becomes unreachable, the adapter's circuit breaker opens after 3 consecutive failures within a 60-second window, and the scheduler marks that source as `degraded`. An alert is emitted via the logging system. We recommend setting up external health checks on these domains independently. The aggregator never hard-codes domain logic; you can override the upstream list at runtime via the admin endpoint `/admin/sources` (authentication required).

## 许可证

This project is distributed under the MIT License. See the `LICENSE` file in the repository root for the full text. You are free to use, modify, merge, publish, distribute, sublicense, and sell copies of the software, subject to the condition that the original copyright notice and this permission notice are retained in all copies or substantial portions of the software.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
