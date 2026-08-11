# LeiSu Resource Catalog

LeiSu Resource Catalog is a high-performance, stateless technical resource aggregation and navigation system designed for developers, researchers, and IT infrastructure teams who need to organize, classify, and rapidly retrieve domain-specific online resources. The project addresses the common challenge of managing fragmented, domain-heavy reference materials by providing a lightweight, file-based cataloging engine that does not require a database backend.

The system operates as a static site generation toolkit that ingests structured resource manifests and produces browsable HTML indices, JSON APIs, and plain-text lookup tables. It is particularly suited for teams that maintain large collections of region-specific or service-oriented URLs and require consistent, version-controlled access patterns without introducing additional runtime dependencies.

## 功能概览

- **Static Resource Indexing** – Parses YAML and JSON manifest files to build an in-memory resource tree with support for tags, categories, and custom metadata fields.

- **Multi-Format Output Generation** – Produces static HTML pages, JSON endpoint responses, and plain-text markdown listings from a single source manifest.

- **Pattern-Based Validation** – Performs syntactic and structural validation on all entered resource identifiers, ensuring protocol consistency and domain format compliance before publication.

- **Batch Import Pipeline** – Supports incremental import of resource lists from CSV, TSV, and line-delimited text files, with automatic duplicate detection and merge conflict resolution.

- **Tag Aggregation Engine** – Computes co-occurrence statistics across resources to generate tag clouds, related resource suggestions, and category heatmaps.

- **Versioned Snapshot Support** – Creates timestamped snapshots of the entire resource catalog, enabling rollback and historical comparison across deployment cycles.

- **Command-Line Query Interface** – Provides a REPL-style interactive mode and scriptable query commands for filtering resources by domain suffix, keyword, or update timestamp.

## 应用场景

- **Technical Documentation Portals** – Project maintainers can use LeiSu Resource Catalog to bundle external reference links, API endpoints, and specification documents into a unified, searchable index that is regenerated on every documentation build.

- **Regional Service Mapping** – Operations teams managing multi-region deployments can maintain a master list of service endpoints across different geographical domains, with automated validation to catch outdated or misformatted entries.

- **Research Bibliography Aggregation** – Academic and research groups can organize large collections of domain-specific preprint servers, institutional repositories, and data sources into a structured catalog that supports tag-based exploration and citation lookup.

- **Offline-First Knowledge Bases** – Teams operating in restricted network environments can use the catalog to maintain a curated list of allowed external resources, with integrity checks and local mirror mappings.

- **CI/CD Integration for URL Governance** – Engineering platforms can embed LeiSu Resource Catalog as a pre-commit hook or CI step to validate all external URLs in code repositories, preventing broken or malformed links from reaching production.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/leisu-dev/leisu-resource-catalog.git
cd leisu-resource-catalog

# Install dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Build the catalog from default manifest
python build.py --manifest manifests/default.yaml --output dist/

# Run the built-in development server to preview
python -m http.server 8080 --directory dist/
```

## 安装要求

| Dependency | Version Requirement | Description |
|------------|---------------------|-------------|
| Python | 3.9 or higher | Core runtime interpreter |
| PyYAML | 6.0 or higher | YAML manifest parsing |
| Jinja2 | 3.1 or higher | HTML template rendering engine |
| Markdown | 3.4 or higher | Markdown to HTML conversion for resource descriptions |
| pytest | 7.0 or higher (development) | Unit and integration test framework |
| black | 23.0 or higher (development) | Code formatting tool |
| mypy | 1.0 or higher (development) | Static type checking |
| requests | 2.28 or higher | Optional HTTP validation for resource liveness checks |
| colorama | 0.4 or higher | Optional terminal color output for CLI mode |

## 文档导航

| Layer | Directory | Questions Addressed |
|-------|-----------|---------------------|
| User Guide | docs/user-guide/ | How to install, configure, and run the catalog builder for personal or team use. |
| Manifest Reference | docs/manifest-spec/ | What fields are available in the YAML manifest, and how to structure multi-level resource hierarchies. |
| API Documentation | docs/api/ | How to consume the generated JSON endpoints programmatically from external tools and scripts. |
| Contributor Handbook | docs/contributing/ | What coding standards, test coverage requirements, and PR workflows apply to project contributions. |
| Deployment Guide | docs/deployment/ | How to deploy the generated static output to CDN, S3, or on-premise web servers with cache strategies. |
| Migration Notes | docs/migration/ | How to upgrade between major versions, including breaking changes and deprecated fields. |

## 资源列表

### 主域名资源

- <code>leisusaishiqianzhan.org.cn</code>
- <code>leisusaiguo.asia</code>
- <code>leisujinrituijian.org.cn</code>
- <code>leisujinrituijian.cn</code>
- <code>leisujishibifen.asia</code>
- <code>leisujishibifen.cn</code>
- <code>leisufenxi.asia</code>

### 资源分类说明

上述域名列表涵盖了本目录当前托管的主要外部引用节点。每个域名对应独立的资源子集或服务端点，具体分类映射关系在 manifests/default.yaml 中定义。所有域名均保留用户提供的原始格式，包括顶级域和协议前缀的缺失状态，系统在处理时自动应用标准化校验规则。

## 项目结构

```
leisu-resource-catalog/
├── build.py                         # Main build orchestration script
├── requirements.txt                 # Production and development dependencies
├── pyproject.toml                   # Project metadata and tool configuration
├── manifests/                       # Source manifest directory
│   ├── default.yaml                 # Primary resource catalog manifest
│   ├── staging.yaml                 # Staging environment overrides
│   └── archive/                     # Historical snapshot manifests
│       └── 2026-01-01-snapshot.yaml
├── src/                             # Core application source code
│   ├── parser/                      # Manifest parsing and validation modules
│   │   ├── yaml_loader.py
│   │   ├── json_loader.py
│   │   └── schema_validator.py
│   ├── engine/                      # Indexing and aggregation logic
│   │   ├── resource_tree.py
│   │   ├── tag_aggregator.py
│   │   └── deduplicator.py
│   ├── generators/                  # Output format generators
│   │   ├── html_renderer.py
│   │   ├── json_serializer.py
│   │   └── markdown_exporter.py
│   └── cli/                         # Command-line interface handlers
│       ├── main_commands.py
│       ├── query_processor.py
│       └── interactive_shell.py
├── templates/                       # Jinja2 HTML templates
│   ├── base.html                    # Base layout template
│   ├── index.html                   # Catalog home page template
│   └── detail.html                  # Per-resource detail page template
├── tests/                           # Test suite
│   ├── unit/                        # Unit tests for individual modules
│   ├── integration/                 # Integration tests for build pipeline
│   └── fixtures/                    # Test data fixtures
├── docs/                            # Project documentation
│   ├── user-guide/
│   ├── manifest-spec/
│   ├── api/
│   ├── contributing/
│   ├── deployment/
│   └── migration/
├── dist/                            # Build output directory (generated)
└── logs/                            # Build and validation logs (generated)
```

## 贡献指南

1. **Fork 仓库并创建功能分支** – 从主仓库 fork 到个人账户，然后创建以 feature/ 或 fix/ 为前缀的分支，例如 feature/tag-export-improvement。

2. **遵循代码规范并运行测试** – 在提交前执行 black 和 mypy 进行格式化和类型检查，运行 pytest 确保所有测试用例通过，新增功能需附带对应的单元测试。

3. **更新文档与示例** – 如果修改了 manifest 结构、CLI 命令或输出格式，需同步更新 docs/ 下对应的用户指南和 API 文档，并在 manifests/ 中提供示例片段。

4. **提交清晰的 Commit 消息** – 采用 Conventional Commits 格式（feat:、fix:、docs:、refactor:），提交信息需简明描述变更内容和原因，避免模糊描述。

5. **创建 Pull Request 并描述变更** – 推送分支后在 GitHub 上创建 PR，填写模板中的变更概述、测试结果和兼容性说明，等待至少一位维护者审核。

## 常见问题

**问：系统是否支持动态数据库后端，例如 PostgreSQL 或 MySQL？**

答：不支持。LeiSu Resource Catalog 明确设计为静态文件生成工具，不依赖任何关系型数据库或 NoSQL 存储。所有资源数据来源于版本控制下的 YAML/JSON 文本文件，构建过程生成纯静态输出。如果用户需要动态查询能力，推荐使用生成的 JSON API 端点配合前端搜索库实现，或者将输出内容导入到 Elasticsearch 等外部检索系统中进行二次索引。

**问：如何处理资源域名变更或失效的情况？**

答：项目提供了可选的 HTTP 存活检查功能，通过在 build.py 中启用 --validate-liveness 参数，系统会发送 HEAD 请求验证每个资源域名的可达性。失效域名会被记录到 logs/validation-errors.log 文件中，构建过程返回非零退出码以中断 CI 流水线。对于域名迁移场景，manifest 支持 redirects 字段，用户可以声明旧域名到新域名的映射关系，系统在生成输出时会自动添加重定向提示。

**问：是否支持多语言资源描述和国际化输出？**

答：当前版本支持在 manifest 中为每个资源条目定义 i18n 字段，包含 zh-CN、en-US 等语言键值对。构建时通过 --lang 参数指定目标语言，HTML 生成器会优先输出对应语言的描述内容，并自动填充 hreflang 标签。但系统不提供自动翻译服务，所有多语言内容需由用户在源文件中预先维护。

## 许可证

MIT License. See the LICENSE file in the repository root for full terms and conditions.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
