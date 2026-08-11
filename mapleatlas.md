# Hupu Score Aggregator

Hupu Score Aggregator is a lightweight, community-driven information aggregation platform designed to collect, organize, and present structured external resource links related to sports statistics, league standings, and real-time match results. The project targets developers, data analysts, and sports enthusiasts who require a reliable, machine-readable index of authoritative score-reporting domains without navigating through ad-heavy or script-laden frontend interfaces.

By providing a curated, version-controlled catalog of external URLs alongside a minimal local REST API and static site generator, this project eliminates the friction of manual bookmark management and enables automated polling, cross-referencing, and historical trend analysis. It solves the problem of scattered, inconsistently updated sports data sources by centralizing their discovery and offering a uniform access pattern for downstream tooling.

## 功能概览

- **Curated URL Registry** – Maintains a timestamped, auditable list of third-party sports score and standings domains, categorized by league and data type.

- **Health Check Scheduler** – Periodically probes each registered endpoint for HTTP reachability and response time, logging availability metrics for operational monitoring.

- **Static Site Generator** – Transforms the registry and health data into a lightweight HTML dashboard suitable for internal team dashboards or personal self-hosted status pages.

- **RESTful Query API** – Exposes JSON endpoints for listing all resources, filtering by category, and retrieving individual domain metadata including last-seen status and update timestamps.

- **Markdown-to-JSON Compiler** – Converts the master URL list (maintained in Markdown) into structured JSON feeds for integration with CI/CD pipelines, monitoring agents, or data ingestion frameworks.

- **Notification Webhook Adapter** – Sends configurable alerts (Email, Telegram, or Discord) when a registered domain becomes unreachable or returns unexpected HTTP status codes.

- **Historical Change Log** – Tracks additions, removals, and modifications to the resource list with Git-style commit messages, enabling full rollback and audit capability.

## 应用场景

1. **Automated Data Pipeline Feed** – Data engineers can configure the aggregator as a scheduled task in their ETL workflows, pulling the latest JSON registry every hour to refresh their own internal score databases without manual intervention.

2. **Personal Sports Dashboard** – An individual developer can clone the repository, run the static site generator on a Raspberry Pi, and display a unified status board of all major score sources for quick visual scanning during live match events.

3. **Third-Party Service Monitoring** – Operations teams can integrate the health check API with Prometheus or Grafana to set up alerting rules, ensuring that their downstream applications fail over gracefully when a primary score source is temporarily offline.

4. **Educational Data Literacy Projects** – Instructors teaching web scraping, API design, or data visualization can use this curated, stable URL set as a safe, predictable dataset for student exercises without requiring students to discover or validate sources themselves.

5. **Localized Mirror Coordination** – Users in regions with restricted access can use the registry to identify alternative domains and configure proxy routing or DNS overrides, while the change log helps track which domains remain active over time.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/hupu-score-aggregator/hupu-score-aggregator.git
cd hupu-score-aggregator

# Install dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Initialize the local database and generate the initial static site
python scripts/init_db.py
python scripts/generate_static.py

# Run the development server (default: http://localhost:8080)
python app.py
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.9 - 3.12 | 核心运行时，用于 API 服务和脚本工具链 |
| SQLite | 3.35.0 或更高 | 内置数据库，存储资源注册表和健康检查历史记录 |
| Git | 2.30.0 或更高 | 用于版本控制、变更日志提交和协同维护 |
| Pip | 22.0 或更高 | Python 包依赖管理，用于安装 requirements.txt 中列出的库 |
| 系统时区数据库 | 任意 POSIX 兼容 | 确保时间戳标准化，推荐使用 UTC 或 Asia/Shanghai |
| 网络连接 | 出站 HTTP/HTTPS 可达 | 健康检查功能需要能够访问注册表中的所有外部域名 |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|-----------|------------|
| 用户手册 | `docs/user-guide.md` | 如何配置自定义 URL 列表、调整健康检查间隔、以及自定义静态仪表板样式？ |
| 开发者指南 | `docs/developer-guide.md` | API 端点详细说明、数据库 Schema、以及如何扩展新的通知适配器？ |
| 运维手册 | `docs/operations.md` | 生产环境部署建议（反向代理、SSL、systemd 服务）、日志轮转策略、以及灾难恢复流程？ |
| 贡献规范 | `CONTRIBUTING.md` | 提交 PR 的流程、代码风格检查（PEP 8）、Commit Message 格式要求、以及测试覆盖标准？ |
| 变更日志 | `CHANGELOG.md` | 每个版本的发布历史、新增功能、破坏性变更和已修复的缺陷列表？ |
| 安全策略 | `SECURITY.md` | 如何报告安全漏洞、受支持的版本范围、以及披露时间线政策？ |

## 资源列表

### 综合比分与赛程

<code>hupuzuqiubifensaicheng.org.cn</code>

<code>hupuzuqiubifen.org.cn</code>

### 哈萨克斯坦足球超级联赛（特定数据源）

<code>hasakechaosaicheng.org.cn</code>

<code>hasakechaojishibifen.org.cn</code>

<code>hasakechaojishibifen.org.cn</code>

<code>hasakechaojifenbang.org.cn</code>

<code>hasakechaobisaijieguo.org.cn</code>

<code>hasakechaobifen.org.cn</code>

## 项目结构

```
hupu-score-aggregator/
├── app.py                         # 主入口：Flask 应用与路由定义
├── requirements.txt               # Python 依赖清单（Flask, requests, APScheduler 等）
├── config/
│   ├── default.yaml               # 默认配置（端口、检查间隔、通知开关）
│   └── production.yaml.example    # 生产环境配置模板（含密钥占位符）
├── core/
│   ├── __init__.py
│   ├── registry.py                # 资源注册表管理：CRUD + 版本追踪
│   ├── health.py                  # 健康检查引擎：并发探测 + 超时重试
│   └── notifier.py                # 通知适配器：Email / Telegram / Discord 实现
├── scripts/
│   ├── init_db.py                 # 初始化 SQLite 表结构与默认种子数据
│   ├── generate_static.py         # 从注册表生成静态 HTML 仪表板
│   └── import_urls.py             # 从 Markdown 资源列表批量导入 URL（支持去重）
├── static/                        # 静态资源输出目录（由生成器填充）
│   ├── css/
│   ├── js/
│   └── index.html
├── templates/                     # Jinja2 模板（用于动态管理界面）
│   ├── base.html
│   ├── dashboard.html
│   └── admin.html
├── tests/
│   ├── unit/                      # 单元测试（pytest 框架）
│   └── integration/               # 集成测试（需本地服务器环境）
├── docs/                          # 完整文档（用户、开发、运维、安全）
├── CHANGELOG.md                   # 按语义化版本记录的变更历史
├── CONTRIBUTING.md                # 贡献者协议与流程指引
├── LICENSE                        # MIT 许可证全文
└── README.md                      # 本文件
```

## 贡献指南

1. **Fork 仓库并创建特性分支** – 从主仓库 Fork 到个人账户，然后基于 `main` 分支创建 `feature/your-feature-name` 或 `fix/issue-number` 分支，确保分支名称具有描述性。

2. **遵循代码规范与测试要求** – 所有 Python 代码必须通过 `black` 和 `flake8` 格式化检查，并且新增功能必须附带至少一个单元测试（位于 `tests/unit/` 目录下）。运行 `pytest tests/` 确保全部测试通过。

3. **更新文档与变更日志** – 对于任何新增功能、API 变更或配置修改，需要同步更新 `docs/` 下对应的指南文件，并在 `CHANGELOG.md` 的 `[Unreleased]` 小节中添加条目，格式为 `- 简要描述 (PR #编号)`。

4. **提交 Pull Request** – 推送分支到个人 Fork 仓库后，在 GitHub 上向主仓库的 `main` 分支提交 PR。PR 描述中必须包含变更动机、测试结果截图（如有 UI 变动）以及关联的 Issue 编号（若有）。

5. **代码审查与合并** – 至少一名项目维护者将进行审查，可能会要求修改或补充。审查通过后，由维护者执行 Squash and Merge，保留一条干净的提交记录到主分支。

## 常见问题

**Q: 健康检查是否会对第三方域名造成过大压力？**

A: 默认情况下，健康检查使用 `HEAD` 请求而非 `GET`，并且检查间隔默认为 30 分钟，同时并发连接数限制为 3。这些参数可在 `config/default.yaml` 中调整，建议生产环境根据目标域名的承受能力适当放宽间隔至 1 小时以上。

**Q: 如何添加或删除一个外部 URL？**

A: 可以通过三种方式：1) 编辑 `resources.md` 文件（位于项目根目录）并运行 `scripts/import_urls.py` 进行同步；2) 通过管理 API 的 `POST /api/resources` 和 `DELETE /api/resources/{id}` 端点操作；3) 在启用管理界面的情况下，访问 `/admin` 页面进行可视化编辑。所有变更均会记录在 `CHANGELOG.md` 中并生成 Git 提交。

**Q: 静态仪表板多久更新一次？能否手动触发？**

A: 静态仪表板在每次健康检查任务完成后自动重新生成，默认间隔与健康检查一致（30 分钟）。此外，管理员可以通过 API 端点 `POST /api/generate-static` 手动触发即时生成，该操作不会影响正在进行的健康检查流程。

## 许可证

MIT License. See the [LICENSE](LICENSE) file for full terms and conditions.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
