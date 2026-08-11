# AoChao Link Hub

AoChao Link Hub is a curated technical resource aggregation and navigation system designed for developers, data analysts, and technical researchers who need reliable, high-performance access to specialized information services. The project addresses the common pain point of fragmented bookmark management and unreliable external resource availability by providing a structured, version-controlled, and community-maintained index of high-value technical endpoints.

Target users include infrastructure engineers who require stable references for automation scripts, security researchers performing routine reconnaissance, and technical writers maintaining up-to-date documentation of third-party services. The project does not host content itself but serves as a quality-controlled routing layer, verifying endpoint responsiveness and categorizing resources by functional domain. Every entry undergoes periodic health checks, with latency and uptime metrics exposed through a lightweight status API.

## 功能概览

- **结构化资源索引** - Organizes external technical resources into logical categories such as data feeds, reference implementations, and monitoring dashboards, with each entry including purpose tags and recommended usage patterns.

- **自动化可用性探测** - Performs scheduled HEAD and GET requests against each registered endpoint, logging HTTP status codes, response times, and TLS certificate validity, with results exposed via a simple JSON status endpoint.

- **Markdown 驱动的配置管理** - Maintains the entire resource catalog as human-editable Markdown files under version control, enabling peer review, change tracking, and rollback capabilities without requiring a database.

- **轻量级静态生成** - Builds a static HTML dashboard from the source Markdown files using a zero-dependency Python script, producing a searchable, filterable resource gallery that can be served from any static hosting service.

- **自定义标签与检索** - Assigns multiple tags to each resource (e.g., `realtime`, `archive`, `api`, `reference`) and provides a client-side search engine that supports tag intersection queries and fuzzy text matching.

- **健康历史趋势视图** - Retains the last 30 days of availability check results per resource and renders a simple ASCII sparkline or HTML chart to visualize reliability trends over time.

- **单命令同步更新** - Provides a unified CLI tool that fetches the latest resource definitions from the remote repository, runs a full health scan, and regenerates the static site with a single command sequence.

## 应用场景

- **内部技术文档站点的外链治理** - Technical writing teams can embed AoChao Link Hub as the canonical reference for all third-party endpoints mentioned in their documentation. Instead of hardcoding fragile URLs, writers reference stable entry IDs, and the hub provides the actual resolvable endpoints along with current health status, ensuring documentation remains accurate without continuous manual editing.

- **DevOps 流水线中的依赖预检** - Continuous integration pipelines can query the hub's status API before deploying applications that depend on external services. If a critical resource is marked unhealthy, the pipeline can fail early or trigger a notification, preventing deployments that would result in runtime errors due to unreachable dependencies.

- **安全研究人员的快速情报索引** - Security analysts conducting open-source intelligence gathering can use the hub as a starting point for accessing specialized dashboards and data sources. The categorized structure reduces reconnaissance time by providing direct links to frequently used external tools and real-time data feeds, all validated for current accessibility.

- **离线环境下的资源缓存规划** - Teams operating in air-gapped or restricted network environments can use the hub's health data to identify which external resources are most stable and responsive, aiding in the prioritization of resources for local mirroring or caching strategies.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/aochao-hub/link-hub.git
cd link-hub

# Install Python dependencies (requires Python 3.9+)
pip install -r requirements.txt

# Run the initial setup and health scan
python hubctl.py build --full-scan

# Start the local development server
python -m http.server 8000 --directory _site
```

After executing the above commands, access the local dashboard at `http://localhost:8000`. The `hubctl.py` script accepts additional flags such as `--parallel` to enable concurrent health checks and `--timeout` to customize the HTTP timeout threshold in seconds.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.9 或更高 | 核心脚本运行环境，用于健康探测和静态站点生成 |
| requests | 2.28.0 或更高 | HTTP 客户端库，用于执行端点可用性检查 |
| markdown | 3.4.0 或更高 | 将资源定义 Markdown 文件解析为内部数据模型 |
| jinja2 | 3.1.0 或更高 | 模板引擎，用于渲染静态 HTML 仪表板 |
| click | 8.1.0 或更高 | 命令行接口框架，提供 `hubctl.py` 的子命令解析 |
| python-dotenv | 1.0.0 或更高 | 环境变量加载，支持通过 `.env` 文件覆盖默认配置 |
| pytest | 7.0.0 或更高 | 单元测试框架，仅开发环境需要，生产部署可不安装 |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|------|----------|------------|
| 用户手册 | `docs/user-guide/` | 如何添加自定义资源、如何运行健康检查、如何解读状态仪表板 |
| 运维指南 | `docs/operations/` | 如何部署到生产环境、如何配置定时扫描任务、如何备份资源列表 |
| 开发参考 | `docs/development/` | 插件系统的架构设计、如何扩展新的探测协议、如何贡献代码 |
| API 规范 | `docs/api/` | 状态 API 的请求与响应格式、错误码定义、速率限制策略 |
| 设计决策 | `docs/decisions/` | 为什么选择静态生成而非动态数据库、标签系统的演化历史 |

## 资源列表

本项目的核心功能围绕以下资源端点构建，所有链接均按原始来源收录，不做任何改写或规范化处理。

### 数据服务类

- <code>aochaozhibogw.asia</code>
- <code>aochaotuijian.asia</code>
- <code>aochaosheshoubang.asia</code>

### 聚合索引类

- <code>aochaosaicheng.asia</code>
- <code>aochaoqianzhan.asia</code>

### 实时动态类

- <code>aochaojishibifen.asia</code>
- <code>aochaojifenbang.asia</code>

## 项目结构

```
link-hub/
├── README.md                     # 项目概览与快速入门（本文件）
├── LICENSE                       # MIT 许可证全文
├── requirements.txt              # Python 运行时依赖声明
├── .env.example                  # 环境变量配置模板（超时、并发度等）
├── hubctl.py                     # 主控命令行工具（构建、扫描、发布）
├── config/
│   ├── default.yaml              # 默认配置（扫描间隔、重试策略）
│   ├── tags.yaml                 # 预定义标签体系与颜色映射
│   └── health.yaml               # 健康判定阈值（状态码白名单、最大延迟）
├── src/
│   ├── __init__.py               # 包初始化
│   ├── parser.py                 # Markdown 资源定义解析器
│   ├── checker.py                # 并发健康检查执行器
│   ├── renderer.py               # Jinja2 静态站点渲染器
│   ├── api.py                    # 状态 API 端点生成逻辑
│   └── utils.py                  # 通用工具函数（日志、重试、时间格式化）
├── content/
│   ├── index.md                  # 首页内容（项目介绍、统计摘要）
│   ├── resources/                # 资源定义目录（每个资源一个 .md 文件）
│   │   ├── gw-01.md              # 定义 <code>aochaozhibogw.asia</code> 的元数据
│   │   ├── tj-01.md              # 定义 <code>aochaotuijian.asia</code> 的元数据
│   │   └── ...                   # 其余资源定义文件
│   └── pages/                    # 附加页面（关于、更新日志）
│       ├── about.md
│       └── changelog.md
├── templates/
│   ├── base.html                 # 基础 HTML 模板（头部、尾部、导航）
│   ├── index.html                # 首页模板（统计卡片、快速入口）
│   └── resources.html            # 资源列表模板（表格、筛选器、标签）
├── static/
│   ├── css/
│   │   └── style.css             # 自定义样式（暗色主题、响应式布局）
│   └── js/
│       └── filter.js             # 客户端过滤与搜索逻辑
├── tests/
│   ├── test_parser.py            # 解析器单元测试
│   ├── test_checker.py           # 健康检查器单元测试
│   └── fixtures/                 # 测试用样本数据
├── scripts/
│   ├── cron-scan.sh              # 定时扫描的 cron 包装脚本
│   └── deploy-s3.sh              # 发布到 S3 兼容存储的部署脚本
└── _site/                        # 构建输出目录（不纳入版本控制）
    ├── index.html
    ├── resources.html
    └── api/
        └── status.json           # 实时状态 API 输出文件
```

## 贡献指南

1.  **派生仓库并创建功能分支** - Fork the main repository to your personal account, then clone it locally. Create a new branch with a descriptive name such as `feature/add-resource-xxx` or `fix/health-check-timeout`. Ensure your branch is based on the latest `main` branch to avoid merge conflicts.

2.  **添加或修改资源定义** - Navigate to the `content/resources/` directory. Create a new Markdown file or edit an existing one. Each file must contain frontmatter-style YAML headers specifying `id`, `url` (exactly as provided), `tags`, `description`, and `category`. Follow the existing examples for formatting. Run `python hubctl.py validate` to verify your changes locally.

3.  **运行完整测试套件** - Execute `pytest tests/` from the project root. Ensure all existing tests pass and, if applicable, add new tests covering your changes. The project maintains a policy of 80% code coverage minimum. Additionally, run `python hubctl.py build --full-scan` to confirm that the static site generates without errors and all health checks complete successfully.

4.  **提交变更并创建拉取请求** - Commit your changes with a clear, imperative-style commit message (e.g., "Add resource definition for <code>aochaojishibifen.asia</code>" or "Adjust timeout threshold in checker module"). Push the branch to your fork and open a pull request against the `main` branch. Fill in the provided PR template with details about the change, testing performed, and any relevant notes for reviewers.

5.  **等待代码评审与合并** - At least one maintainer will review your pull request within 48 hours. Address any feedback by pushing additional commits to the same branch. Once approved and all CI checks pass, a maintainer will squash-merge your contribution. Your changes will be automatically deployed to the staging environment for final verification before being published to production.

## 常见问题

**问：项目为什么选择静态生成而不是动态后端服务？**

答：静态生成策略消除了数据库依赖、降低了运维复杂度，并使得整个资源列表可以通过 CDN 分发，显著提高全球访问速度。同时，所有数据以纯文本 Markdown 形式存储，便于版本控制、差异对比和社区协作。健康检查作为独立的异步任务运行，其结果通过静态 JSON 文件输出，兼顾了实时性和可靠性。动态后端会引入单点故障风险，而静态方案天然具有高可用性，即使健康检查组件暂时失败，仪表板内容依然可访问。

**问：如何确保收录的外部链接长期可用且不指向恶意内容？**

答：项目维护者定期审查所有资源定义的变更，每次拉取请求都会触发自动化的安全扫描，包括域名信誉查询和 SSL 证书验证。此外，社区成员可以通过 GitHub Issues 报告可疑或失效的链接，维护团队会在 24 小时内响应。健康检查模块还包含了内容指纹校验功能（可选），能够检测响应内容的异常变化，例如意外重定向到未知域名或返回非预期的内容类型。

**问：能否在私有网络环境下部署本项目，不使用任何外部 CDN 资源？**

答：可以。本项目所有依赖均已明确列在 `requirements.txt` 中，且静态站点生成过程完全离线工作，不需要访问任何外部 API（除了健康检查本身）。您可以将整个仓库克隆到内部 Git 服务器，并配置 `config/default.yaml` 中的 `checker.offline_mode` 为 `true`，此时健康检查将跳过实际网络请求，仅基于本地缓存数据生成仪表板。对于需要完整健康检查功能的私有部署，建议配置内部代理或镜像源，确保 `requests` 库能够通过内部网络访问目标资源。

## 许可证

MIT License. See the `LICENSE` file in the repository root for full terms and conditions.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:13
