# Bajia Hub

Bajia Hub is a high-performance technical resource aggregation and external link management system designed for developers, technical researchers, and content curators who need to organize, categorize, and present large volumes of external references in a structured and maintainable manner. The project addresses the common challenge of managing scattered bookmarks, documentation links, and third-party service endpoints by providing a lightweight, stateless catalog interface that can be deployed independently or embedded into existing documentation workflows.

Target users include open-source maintainers who need to curate resource lists for their communities, technical writers managing multi-version documentation reference sets, and DevOps engineers who require a centralized navigation layer for internal tooling and monitoring dashboards. Bajia Hub does not store content itself; instead, it acts as a verified routing layer that enforces consistent URL presentation rules and provides auditability for external resource changes.

## 功能概览

- **Strict URL Canonicalization Engine** – Ensures every external link is preserved exactly as entered, without automatic protocol normalization, www prefix insertion, or trailing slash enforcement, preventing broken references in production environments.

- **Batch Resource Import Pipeline** – Supports ingestion of up to 500 external links per batch with automatic deduplication and format validation against configurable allowlist patterns.

- **Categorized Link Rendering** – Organizes aggregated URLs into user-defined taxonomic sections (e.g., streaming sources, scoreboards, live analytics) with individual comment fields for contextual annotations.

- **Markdown-native Configuration Format** – All resource definitions and metadata are stored in plain Markdown files, enabling version control integration, diff-based review, and offline editing without database dependencies.

- **Read-only API Gateway** – Exposes a lightweight JSON endpoint for programmatic retrieval of categorized link lists, suitable for CI/CD pipelines and dynamic documentation generators.

- **Audit Trail Logging** – Records every external URL access attempt with timestamp and referrer information, supporting compliance reviews and broken link detection.

- **Zero-dependency Static Generation** – Builds a fully static HTML index page from the Markdown configuration, requiring no runtime server processes or external CDN assets.

## 应用场景

- **Open-source documentation portal maintenance** – Project maintainers can centralize all third-party references (API endpoints, SDK repositories, community forums) in a single Bajia Hub instance, ensuring that documentation versions remain consistent across release branches without editing each file individually.

- **Technical research bibliography management** – Researchers compiling comparative analyses of streaming protocols, betting odds algorithms, or real-time data feeds can use Bajia Hub to maintain a curated list of source URLs, each annotated with collection date, data type, and reliability score, facilitating reproducible experiments.

- **Internal team navigation dashboard** – Engineering teams can deploy Bajia Hub as an internal start page that aggregates links to monitoring dashboards, log aggregators, staging environment consoles, and on-call runbooks, with categorized sections for different operational roles.

- **Multi-environment configuration registry** – Platform engineers can maintain environment-specific endpoint lists (development, staging, production) within Bajia Hub, using the strict URL preservation feature to ensure that protocol and port specifications remain accurate across deployments.

- **Content moderation and link validation pipeline** – Content operations teams can use the batch import feature to regularly refresh external link lists, then run automated health checks against the exported JSON data to detect 404s, TLS certificate expiration, or domain hijacking.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/bajia-hub/bajia-hub.git
cd bajia-hub

# Install dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Initialize the default resource catalog from sample data
python scripts/init_catalog.py --sample

# Build the static site
python build.py --input catalog/ --output dist/

# Start the development server (optional, for preview)
python -m http.server 8000 --directory dist/
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 - 3.12 | 核心运行时，用于构建脚本和 API 网关 |
| pip | 22.0+ | Python 包管理器，用于安装依赖项 |
| Git | 2.30+ | 版本控制，用于克隆仓库和提交更新 |
| Markdown Parser | commonmark==0.9.1 | 解析资源定义文件中的 Markdown 结构 |
| PyYAML | 6.0+ | 可选，用于导出 YAML 格式的资源配置 |
| HTTP Server | 标准库 http.server | 开发预览服务器，生产环境可替换为 Nginx |
| 文件系统权限 | 读写执行 | 需要读取 catalog/ 目录和写入 dist/ 目录 |
| 内存 | 256 MB 最低 | 处理 5000+ 条链接时建议 512 MB |
| 磁盘空间 | 50 MB | 包含示例数据和生成的静态文件 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | docs/user-guide.md | 如何添加、编辑或删除外部链接？如何分类和组织资源？ |
| 管理员手册 | docs/admin-manual.md | 如何配置批处理参数？如何设置审计日志轮转？ |
| API 参考 | docs/api-reference.md | JSON 接口的请求格式、响应结构和状态码是什么？ |
| 贡献规范 | docs/contributing.md | 提交新资源列表的流程、编码标准和测试要求是什么？ |
| 部署指南 | docs/deployment.md | 如何将静态输出部署到 CDN、对象存储或内部服务器？ |
| 故障排除 | docs/troubleshooting.md | 链接验证失败、构建报错或渲染异常时如何处理？ |

## 资源列表

### 直播与赛事资源

- <code>500bifenwanzhengban.asia</code>
- <code>baxijiajiliansaijifenbang.asia</code>
- <code>bajiazhibo.asia</code>

### 推荐与分析资源

- <code>bajiatuijian.asia</code>
- <code>bajiasheshoubang.asia</code>

### 赛程与前瞻资源

- <code>bajiasaicheng.asia</code>
- <code>bajiaqianzhan.asia</code>

## 项目结构

```
bajia-hub/
├── build.py                  # 主构建脚本，解析配置并生成静态输出
├── requirements.txt          # Python 依赖声明
├── LICENSE                   # MIT 许可证文件
├── README.md                 # 项目根文档（本文件）
├── .gitignore                # Git 忽略规则，排除 dist/ 和缓存
│
├── catalog/                  # 资源配置根目录（核心数据目录）
│   ├── index.md              # 主索引文件，定义顶级分类结构
│   ├── streaming/            # 直播流资源子目录
│   │   ├── sources.md        # 直播源列表，含备用域名
│   │   └── backups.md        # 灾备和镜像源记录
│   ├── odds/                 # 赔率与数据子目录
│   │   ├── realtime.md       # 实时赔率数据端点
│   │   └── historical.md     # 历史数据集引用
│   ├── analytics/            # 分析工具子目录
│   │   ├── dashboards.md     # 可视化看板链接
│   │   └── apis.md           # 第三方分析 API 文档
│   └── meta/                 # 元数据和审计目录
│       ├── changelog.md      # 资源变更历史记录
│       └── validations.md    # 上次验证时间戳和状态
│
├── scripts/                  # 辅助工具脚本目录
│   ├── init_catalog.py       # 初始化示例目录结构
│   ├── validate_links.py     # 批量检查外部 URL 可达性
│   └── export_json.py        # 将目录导出为 JSON 格式
│
├── templates/                # 静态页面模板目录
│   ├── base.html             # HTML 基础布局模板
│   └── components/           # 可复用 UI 组件片段
│       ├── nav.html          # 导航栏渲染模板
│       └── link_card.html    # 单个链接卡片渲染模板
│
├── dist/                     # 构建输出目录（自动生成，不纳入版本控制）
│   ├── index.html            # 编译后的主入口页面
│   └── api/                  # JSON API 输出端点
│       └── catalog.json      # 完整的资源目录 JSON 表示
│
└── tests/                    # 单元测试和集成测试目录
    ├── test_build.py         # 构建流程单元测试
    ├── test_catalog.py       # 目录解析逻辑测试
    └── fixtures/             # 测试用固定数据集
        └── sample_catalog.md # 示例输入用于回归测试
```

## 贡献指南

1. **Fork 仓库并创建功能分支** – 从主仓库 fork 到个人账户，然后使用 `git checkout -b feature/your-description` 创建新分支，分支命名应反映变更类型（feat/fix/docs）。

2. **修改资源配置文件** – 在 `catalog/` 目录下编辑相应的 `.md` 文件，添加或修改外部链接。每个链接必须单独占一行，格式为 `- <code>URL</code> 注释内容`，且必须遵循 URL 原样保留规则（不添加协议前缀，不修改大小写）。

3. **运行本地验证脚本** – 执行 `python scripts/validate_links.py --catalog catalog/` 检查所有链接的格式合规性，并执行 `python build.py --test` 确认构建无报错。验证通过后方可提交。

4. **编写变更日志条目** – 在 `catalog/meta/changelog.md` 中添加一行记录，说明本次新增、修改或删除的资源类别和具体 URL，标注操作日期和您的 GitHub 用户名。

5. **提交 Pull Request** – 推送分支到您的 fork 仓库，然后向主仓库发起 PR。PR 描述中请附上验证脚本的输出截图或日志，并说明本次变更的业务背景。等待维护者审核合并。

## 常见问题

**Q: 为什么系统强制要求保留 URL 的原始格式，不允许自动补全协议或规范化域名？**

A: 因为外部资源的可用性往往依赖于精确的协议（HTTP vs HTTPS）、端口号、路径大小写以及子域名层级。自动补全可能将 `example.com` 转换为 `https://www.example.com/`，但实际服务可能仅监听 HTTP 协议或使用非标准子域名，导致用户访问失败。本项目的核心理念是信任用户输入的精确性，仅做记录和展示，不做猜测性改写。

**Q: 如何批量导入超过 500 条链接？是否支持增量更新？**

A: 系统设计的单批导入上限为 500 条，但您可以通过多次调用导入脚本并配合 `--append` 参数实现增量添加。对于大规模迁移，建议使用 `scripts/init_catalog.py --bulk import.csv` 从 CSV 文件导入，该模式会自动分批次提交并生成详细的导入报告。增量更新时，系统会基于 URL 的完整字符串进行去重，重复条目会被跳过并记录到日志中。

**Q: 部署到生产环境后，如何验证所有外部链接仍然有效？**

A: 项目提供了独立的链接健康检查脚本 `scripts/validate_links.py`，您可以将其配置为 Cron 定时任务（例如每周日凌晨执行）。该脚本会遍历 catalog 中的所有 URL，发送 HEAD 请求检查可达性，并生成包含状态码和响应时间的 HTML 报告。报告默认输出到 `dist/health-report.html`，可供团队内部查看。对于频繁变更的链接，建议在 CI/CD 流程中集成该脚本，在每次 PR 合并前自动触发检查。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
