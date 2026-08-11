# Daxiang Navigator

Daxiang Navigator is a curated technical resource aggregation and external link management system designed for open-source developers, technical researchers, and IT infrastructure teams. It addresses the common challenge of scattered, unmaintained bookmark collections by providing a structured, version-controlled, and collaboratively maintained repository of high-value technical references, official documentation portals, and industry-specific resource hubs.

The project targets teams and individuals who need a reliable, machine-readable, and human-accessible index of external technical assets. By treating external URLs as first-class resources with metadata, status tracking, and categorization, Daxiang Navigator transforms a static list of links into a living knowledge base that can be integrated into CI/CD pipelines, documentation generators, and internal developer portals.

## 功能概览

- **Structured Resource Indexing** - Organizes external URLs into a hierarchical category tree with support for tags, descriptions, and update timestamps, enabling fast lookup and systematic review.

- **Automated Link Health Checking** - Periodically validates all stored URLs for HTTP status codes, SSL certificate validity, and DNS resolution, flagging broken or redirected links with detailed logs.

- **Markdown-Based Documentation Generation** - Consumes the resource index to produce human-readable README files, site maps, and category overviews in pure Markdown, suitable for static site generators or GitHub rendering.

- **Multi-Format Export** - Supports export of the resource list as JSON, YAML, CSV, and plain text, allowing seamless integration with monitoring tools, bookmarking services, and internal wikis.

- **Batch Import and Deduplication** - Provides CLI commands to import URL lists from CSV files or standard input, with automatic deduplication, normalization, and conflict resolution based on configurable rules.

- **Change History and Audit Logging** - Maintains a Git-friendly changelog of all additions, removals, and modifications to the resource set, with author attribution and timestamp tracking for compliance and review purposes.

- **Tag-Based Filtering and Search** - Implements a lightweight full-text and tag-based search engine over the resource metadata, enabling rapid discovery of relevant links by topic, domain, or usage context.

- **Custom Metadata Fields** - Allows users to attach arbitrary key-value metadata to each resource entry, such as maintenance responsibility, priority level, geographic region, or internal project codes.

## 应用场景

- **Internal Developer Portal Maintenance** - Platform engineering teams can use Daxiang Navigator to maintain a centralized, version-controlled registry of all external services, APIs, and documentation sites relied upon by internal microservices. The health check feature provides early warning of external dependency failures.

- **Open-Source Project Documentation Enrichment** - Project maintainers can embed Daxiang Navigator as a submodule or data source to automatically generate "Related Resources" or "External References" sections in their project README, ensuring that all external links are consistently formatted and periodically verified.

- **Technical Research and Benchmarking** - Researchers evaluating industry trends, competing platforms, or regulatory compliance resources can organize large collections of reference URLs into categorized lists, export them for sharing with peers, and track changes over time as new standards emerge.

- **Offline Documentation Mirroring Preparation** - Teams operating in air-gapped or limited-connectivity environments can use the exported JSON or CSV outputs to script offline mirroring operations, ensuring that critical external resources are available locally without manual intervention.

- **Compliance and Regulatory Reference Tracking** - Compliance officers can maintain a structured inventory of regulatory bodies, standards organizations, and official policy documents, with audit logs proving that the listed resources were reviewed and validated at specific points in time.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/your-org/daxiang-navigator.git
cd daxiang-navigator

# Install dependencies (Python 3.10+ required)
pip install -r requirements.txt

# Initialize the local resource database and import the default catalog
python navigator.py init
python navigator.py import --source data/default_catalog.csv

# Run the health check for all resources
python navigator.py check --all

# Generate the consolidated README and category index
python navigator.py generate --output README.md --format markdown

# Start the lightweight web dashboard (optional)
python navigator.py serve --port 8080
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.10 或更高 | 核心运行环境，用于 CLI 工具和健康检查脚本 |
| pip | 23.0 或更高 | Python 包管理器，用于安装依赖库 |
| Git | 2.30 或更高 | 版本控制，用于克隆仓库和提交变更记录 |
| requests | 2.31.0 | HTTP 客户端库，执行 URL 健康检查和状态码验证 |
| pyyaml | 6.0 | YAML 解析器，支持 YAML 格式的资源导入导出 |
| click | 8.1.0 | CLI 框架，提供命令行参数解析和交互式提示 |
| python-dotenv | 1.0.0 | 环境变量管理，用于配置代理、超时等运行时参数 |
| pytest | 7.4.0 | 单元测试框架（开发依赖），用于运行测试套件 |
| black | 24.0.0 | 代码格式化工具（开发依赖），保持代码风格一致 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | docs/user-guide/ | 如何安装、配置、日常使用 CLI 命令，以及如何导入导出资源列表 |
| 管理员手册 | docs/admin-manual/ | 如何部署健康检查定时任务、配置通知、管理多用户权限和审计日志 |
| 开发者文档 | docs/developer-guide/ | 如何扩展自定义元数据字段、添加新的导出格式、贡献核心代码 |
| API 参考 | docs/api-reference/ | 内部 Python API 的模块说明、类结构、函数签名，用于二次开发 |
| 运维手册 | docs/ops-guide/ | 如何监控资源可用性、处理失效链接、优化大规模资源集的性能 |
| 贡献者指南 | CONTRIBUTING.md | 贡献流程、编码规范、提交信息格式、测试要求和 PR 审查标准 |

## 资源列表

以下为项目初始收录的外部资源，按类别分组。所有链接均保持原始格式，未做任何修改。

### 综合信息门户

- <code>daxiangjiaozonghe.org.cn</code>

### 第三方测试与验证

- <code>sanbangcheshiwang.org.cn</code>

### 多媒体与内容平台

- <code>oumeidiyishipin.org.cn</code>
- <code>shoujizaixianguankannidongde.org.cn</code>

### 语言与字符集工具

- <code>zhongwenzimuzipai.org.cn</code>

### 专业技术资料

- <code>ribendaxiangjiao.org.cn</code>

### 软件与工具下载

- <code>liumangruanjianxiazai.org.cn</code>

## 项目结构

```
daxiang-navigator/
├── data/                                 # 数据目录，存放资源索引和缓存
│   ├── default_catalog.csv               # 默认资源清单（CSV格式）
│   ├── resources.json                    # 规范化后的主索引（JSON）
│   └── cache/                            # 健康检查结果缓存目录
│       └── status_20260811.json          # 按日期保存的检查快照
├── docs/                                 # 完整文档目录
│   ├── user-guide/                       # 用户指南章节
│   │   ├── installation.md               # 详细安装步骤
│   │   └── cli-commands.md               # 所有CLI命令参考
│   ├── admin-manual/                     # 管理员手册
│   │   ├── scheduling.md                 # 定时任务配置
│   │   └── notifications.md              # 告警与通知设置
│   └── developer-guide/                  # 开发者文档
│       ├── architecture.md               # 系统架构设计
│       └── plugin-system.md              # 插件开发接口
├── src/                                  # 源代码主目录
│   ├── core/                             # 核心模块
│   │   ├── resource.py                   # Resource类定义与序列化
│   │   ├── indexer.py                    # 索引构建与查询引擎
│   │   └── validator.py                  # URL验证与规范化逻辑
│   ├── cli/                              # 命令行接口实现
│   │   ├── main.py                       # 入口点与命令分发
│   │   ├── import_cmd.py                 # import子命令实现
│   │   └── check_cmd.py                  # check子命令实现
│   ├── exporters/                        # 导出器模块
│   │   ├── markdown.py                   # Markdown格式生成器
│   │   ├── json_exporter.py              # JSON导出器
│   │   └── csv_exporter.py               # CSV导出器
│   └── utils/                            # 通用工具函数
│       ├── http_client.py                # 自定义HTTP客户端（带重试）
│       └── logger.py                     # 日志配置与格式化
├── tests/                                # 单元测试与集成测试
│   ├── unit/                             # 单元测试用例
│   │   ├── test_resource.py              # 测试Resource类
│   │   └── test_validator.py             # 测试URL验证函数
│   └── integration/                      # 集成测试
│       └── test_cli_flow.py              # 端到端CLI流程测试
├── scripts/                              # 运维与辅助脚本
│   ├── daily_check.sh                    # 每日健康检查Shell脚本
│   └── backup_index.sh                   # 索引备份脚本
├── requirements.txt                      # 生产环境依赖列表
├── requirements-dev.txt                  # 开发环境额外依赖
├── setup.py                              # Python包安装配置
├── README.md                             # 项目主说明文档（本文件）
└── CONTRIBUTING.md                       # 详细贡献者指南
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** - 从主仓库 Fork 到个人账户，然后基于 `main` 分支创建以 `feature/` 或 `fix/` 为前缀的命名分支，例如 `feature/add-csv-validator`。

2.  **遵循代码风格和测试标准** - 所有 Python 代码必须通过 Black 格式化检查，并且新功能需包含对应的单元测试，测试覆盖率不得低于 85%。运行 `pytest tests/` 确认全部用例通过。

3.  **提交变更并编写清晰的提交信息** - 提交信息需遵循约定式提交格式（Conventional Commits），例如 `feat: add batch import progress bar` 或 `fix: handle redirect loop in health check`。每个提交应逻辑独立，便于审查。

4.  **更新相关文档和示例** - 如果新增功能或修改现有行为，必须同步更新 `docs/` 目录下的相应文档，并确保 `README.md` 中的快速开始示例仍可正常运行。

5.  **创建 Pull Request 并等待审核** - 推送分支到个人 Fork 后，向主仓库的 `main` 分支发起 Pull Request。在 PR 描述中详细说明变更内容、测试结果和影响范围。至少需要一名项目维护者的批准方可合并。

## 常见问题

**Q: 如何处理健康检查中的误报，例如临时网络抖动导致的超时？**

A: 项目提供了重试机制和超时配置。您可以在根目录的 `.env` 文件中设置 `CHECK_RETRIES=3` 和 `CHECK_TIMEOUT=10`（单位秒）。此外，健康检查命令支持 `--retry-delay` 参数，可自定义重试间隔。对于持续失败的链接，系统会记录失败次数，只有当连续失败达到 `MAX_CONSECUTIVE_FAILURES`（默认 3）时才会标记为异常，避免瞬时波动造成干扰。

**Q: 是否可以自定义资源列表的元数据字段，例如添加“所属团队”或“SLA级别”？**

A: 可以。Resource 类的 `metadata` 字段接受任意 JSON 可序列化的键值对。您可以通过 `navigator.py update --id <resource-id> --metadata '{"team": "platform", "sla": "critical"}'` 命令动态添加或修改。这些自定义字段会保留在 JSON 导出中，并且可以在生成 Markdown 时通过自定义模板渲染，方便集成到内部文档体系。

**Q: 如何将现有的浏览器书签或 Pocket 列表批量导入到 Daxiang Navigator？**

A: 项目支持从 CSV 文件批量导入，并提供了转换脚本 `scripts/convert_bookmarks.py`。您可以从浏览器导出书签为 HTML 格式，然后使用该脚本转换为符合导入规范的 CSV。也支持通过 `navigator.py import --from-pocket pocket_export.html` 直接解析 Pocket 导出的 HTML 文件。导入前系统会自动去重并提示冲突项，您可以选择跳过、覆盖或保留两者。

## 许可证

MIT License

Copyright (c) 2026 Daxiang Navigator Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:11
