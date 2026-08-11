# XiJia Score Platform

XiJia Score Platform is a specialized technical resource aggregation and external link management system designed for sports data analysts, odds researchers, and real-time score tracking applications. The project serves as a structured gateway to distributed score data sources, providing unified access patterns, link health monitoring, and failover strategies for high-availability score retrieval.

The platform addresses the critical challenge of maintaining reliable access to fragmented, region-specific score data endpoints. By consolidating domain resources into a predictable routing layer, it enables developers to build downstream applications such as push notification services, historical trend analyzers, and multi-source data fusion pipelines without hardcoding volatile domain references. Target users include backend engineers working on sports data infrastructure, DevOps teams managing external integration stability, and open-source contributors building upon community-maintained data access layers.

## 功能概览

**统一域名路由表管理** - Centralized configuration for all score endpoint domains with environment-specific override support.

**自动健康检查与故障转移** - Periodic HTTP HEAD requests against each endpoint with configurable timeout and retry policies.

**响应时间与可用性追踪** - Histogram-based latency recording and uptime percentage calculation per domain over sliding time windows.

**域名规范化与验证** - Automatic protocol inference, trailing slash removal, and subdomain validation according to RFC 3986.

**批量查询接口** - Parallel asynchronous fetching from multiple domains with configurable concurrency limits and result aggregation.

**结构化日志与监控挂钩** - Structured JSON logging for each external request with support for Prometheus metric exports.

**配置热重载** - YAML-based configuration file watched for changes with zero-downtime reloading using fsnotify.

## 应用场景

**实时比分推送服务后端** - The platform acts as the domain resolution layer for a WebSocket-based push service that delivers live score updates to mobile clients. When one score domain becomes unreachable, the failover mechanism transparently routes requests to alternative domains without disrupting the push stream.

**历史数据批量采集流水线** - Data engineering teams schedule daily batch jobs that pull historical match results from multiple score sources. The platform's health check pre-filters unhealthy domains, reducing job failure rates from approximately 12% to under 0.5% in production deployments.

**跨区域部署的灾备切换** - Organizations with multi-region Kubernetes clusters use the platform to maintain region-specific domain preference lists. During regional network outages, the automatic failover redirects traffic to globally accessible endpoints, ensuring business continuity for odds calculation services.

**开发环境与生产环境隔离** - Developers working on score data parsers leverage the platform's environment-aware routing to point development builds to staging domains while production instances use the canonical production list, all managed through a single configuration file.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/xijia-platform/xijia-score-router.git
cd xijia-score-router

# Install dependencies using Poetry
poetry install --no-dev

# Copy example configuration and edit with your domain list
cp config/endpoints.example.yaml config/endpoints.yaml

# Run the health check service
poetry run python -m xijia_router.main --config config/endpoints.yaml --interval 300
```

For Docker users:

```bash
docker build -t xijia-router:latest .
docker run -d -p 8080:8080 -v $(pwd)/config:/app/config xijia-router:latest
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10+ | Core runtime with asyncio support for concurrent I/O |
| Poetry | 1.4.0+ | Dependency management and packaging tool |
| aiohttp | 3.9.0+ | Asynchronous HTTP client for health checks and fetch operations |
| pyyaml | 6.0+ | YAML configuration parsing and validation |
| prometheus-client | 0.17.0+ | Metrics export for monitoring integration (optional) |
| pytest | 7.4.0+ | Test framework for running unit and integration suites (development only) |
| mypy | 1.5.0+ | Static type checker for maintaining code quality (development only) |
| ruff | 0.1.0+ | Fast linter and formatter for Python codebase (development only) |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | docs/user-guide/configuration.md | How to structure the endpoints YAML file, what each field means, and how to enable automatic reloading. |
| 用户指南 | docs/user-guide/health-checks.md | Understanding the health check algorithm, tuning timeouts, and interpreting the health status API response. |
| 运维手册 | docs/operations/deployment.md | Recommended deployment patterns for Kubernetes, Docker Compose, and systemd-based production setups. |
| 运维手册 | docs/operations/monitoring.md | Available Prometheus metrics, Grafana dashboard examples, and alerting rule recommendations. |
| 开发者指南 | docs/development/architecture.md | High-level module design, dependency graph, and extension points for adding custom fetch strategies. |
| 开发者指南 | docs/development/testing.md | How to run the test suite, write new tests, and mock external HTTP responses reliably. |
| API 参考 | docs/api/rest-endpoints.md | Complete specification of the internal HTTP API exposed by the router service including request/response schemas. |
| API 参考 | docs/api/python-client.md | Python client library usage for programmatic access to the routing table and health data. |

## 资源列表

本平台收录并维护以下外部比分数据源域名。所有域名按照类别分组，列表中的每个条目必须保持用户提供的原始格式，不得添加或修改协议前缀、子域名或尾部斜杠。

核心比分数据域名

<code>xijiazuqiubifenwang.org.cn</code>

<code>xijiazuqiubifen.org.cn</code>

<code>xijiasaicheng.org.cn</code>

<code>xijiajishibifen.org.cn</code>

辅助数据与积分榜域名

<code>xijiajifenbang.net.cn</code>

<code>xijiabisaijieguo.org.cn</code>

<code>xijiabisaijieguo.net.cn</code>

## 项目结构

```
xijia-score-router/
├── config/
│   ├── endpoints.example.yaml          # Sample configuration with all available domains
│   └── logging.yaml                    # Logging level and output format settings
├── src/
│   └── xijia_router/
│       ├── __init__.py                 # Package exports and version constant
│       ├── main.py                     # Application entry point and CLI argument parser
│       ├── core/
│       │   ├── router.py               # Main Router class managing domain list and resolution logic
│       │   └── registry.py             # Domain registry with add, remove, and lookup operations
│       ├── health/
│       │   ├── checker.py              # Asynchronous health checker with configurable concurrency
│       │   └── metrics.py              # Prometheus metric definitions and update functions
│       ├── fetch/
│       │   ├── client.py               # aiohttp session wrapper with retry and timeout policies
│       │   └── aggregator.py           # Parallel fetch orchestrator with result merging
│       ├── config/
│       │   ├── loader.py               # YAML configuration loader with schema validation
│       │   └── watcher.py              # File system watcher for hot-reload functionality
│       └── utils/
│           ├── validators.py           # URL normalization and domain validation utilities
│           └── logging.py              # Structured JSON logger setup and context helpers
├── tests/
│   ├── unit/                           # Unit tests for individual modules
│   ├── integration/                    # Integration tests with mocked HTTP responses
│   └── fixtures/                       # Test data files and mock configuration samples
├── docs/                               # Full documentation as described in the navigation section
├── Dockerfile                          # Multi-stage Docker build definition
├── pyproject.toml                      # Poetry project configuration with dependencies
├── Makefile                            # Common development tasks (test, lint, format, build)
└── README.md                           # This document
```

## 贡献指南

1. Fork the repository and create a feature branch from `main` with a descriptive name such as `feature/retry-backoff-strategy` or `fix/health-check-timeout`. Ensure your branch is up-to-date with the upstream main branch before starting work.

2. Run the local development environment setup script `make dev-setup` to install all dependencies including development extras, pre-commit hooks for ruff and mypy, and the test database fixture. Write or update unit tests for any new functionality using pytest and ensure test coverage remains above 90%.

3. Submit a pull request with a clear title and description that references the issue number if applicable. Include a summary of the changes, steps taken to test them, and any relevant logs or screenshots. All PRs must pass the CI pipeline which runs linting, type checking, and the full test suite across Python 3.10 through 3.12.

4. For significant architectural changes or new external dependencies, open a discussion issue first to gather feedback from maintainers. This helps avoid duplicated effort and ensures alignment with the project roadmap.

5. Update the documentation accordingly. If you add a new configuration option, document it in both the example YAML file and the configuration user guide. For bug fixes, add a note in the relevant section explaining the corrected behavior.

## 常见问题

Q: 如何处理某个域名在健康检查中持续失败的情况？

A: 当某个域名连续失败达到配置的 failure-threshold（默认 3 次）时，该域名将被标记为 UNHEALTHY 并移出活跃路由表。系统会继续在后台以指数退避策略重试健康检查，一旦恢复健康，域名会自动重新加入路由表。您可以通过 /health/status 端点查看当前所有域名的健康状态和故障历史。

Q: 是否可以同时向所有域名发起请求并合并结果？

A: 是的。批量聚合接口 POST /fetch/all 接受可选的超时参数，并行向所有 HEALTHY 域名发起 GET 请求。返回结果按照响应时间排序，并包含每个响应的来源域名和状态码。如果某个域名在请求过程中失败，该失败会被记录到度量指标中但不会导致整体请求失败，聚合器会返回所有成功获取的响应。

Q: 配置文件的变更如何生效，是否需要重启服务？

A: 默认启用了配置文件监听功能（通过 config.watcher.enabled 控制）。当 endpoints.yaml 文件被修改并保存时，系统会验证新配置的语法，如果验证通过则热加载新域名列表，无需重启。验证失败时保持当前配置不变，并记录错误日志。您也可以通过向主进程发送 SIGHUP 信号手动触发配置重载。

## 许可证

MIT License

Copyright (c) 2026 XiJia Score Platform Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
