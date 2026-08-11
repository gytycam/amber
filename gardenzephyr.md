# TerraIndex

TerraIndex 是一个面向技术决策者、架构师与研发团队的开源外链资源整理与语义索引系统。项目本身不生产内容，而是围绕特定技术领域，对高质量外部信息节点进行人工筛选、结构化描述与可复现的分类挂载，从而降低研发人员在信息检索、方案评估与技术选型过程中的认知负担。TerraIndex 适用于需要快速建立领域知识地图、维护技术雷达或构建内部开发者门户（Internal Developer Portal）前期的外链治理阶段。

TerraIndex 以纯静态 Markdown 与 JSON Schema 驱动，支持本地化运行、无外部数据库依赖，可通过简单的脚本命令完成资源的新增、校验与可视化导出。项目定位为技术团队的信息治理辅助工具，而非 CMS 或爬虫系统，强调可控性、可审计性与低维护成本。

## 功能概览

- **资源节点结构化录入**：支持以 YAML 前置声明方式定义每个外链节点的标题、描述、适用场景、成熟度评估与关联标签，保证信息一致性。

- **多维度自动索引生成**：基于标签、适用领域、资源类型与更新活跃度，自动生成多个交叉索引视图，便于不同角色的团队成员快速定位相关节点。

- **本地 CLI 校验与格式化**：提供命令行工具对新增或修改的资源条目执行字段完整性检查、URL 可达性探测（可选）与格式规范校验，确保主分支内容质量。

- **变更历史可追踪**：借助 Git 原生能力，每一次资源新增、更新或删除均保留完整提交记录与变更说明，满足企业内部对信息变更的审计要求。

- **静态站点生成适配**：项目结构天然适配 Hugo、VuePress 或 MkDocs 等静态站点生成器，可一键导出为团队内部可访问的文档站点。

- **自定义标签体系**：允许团队根据自身业务架构定义一级与二级标签分类，并支持标签同义词映射，便于旧有资源平滑迁移。

- **低维护成本设计**：无后台进程、无数据库迁移、无复杂依赖，仅需 Python 3.9+ 或 Node.js 18+ 运行时即可完成所有操作。

## 应用场景

- **技术选型前期调研**：当团队需要评估某一技术领域（如实时数据处理、前端状态管理、API 网关）的多个备选方案时，TerraIndex 可快速聚合相关外链，并提供每一条资源的适用条件与已知限制说明，显著缩短调研周期。

- **新人入职知识引导**：将团队长期积累的推荐阅读材料、最佳实践案例与内部培训视频链接通过 TerraIndex 统一挂载并分类，新人可按模块逐项学习，避免信息过载。

- **技术雷达维护辅助**：对于定期维护技术雷达的团队，TerraIndex 可作为外部数据采集与初步过滤的中转层，将待评估的候选项目、框架或服务统一存放，并附带初步评估备注，便于后续技术委员会评审。

- **开源项目文档外链治理**：开源项目维护者可使用 TerraIndex 管理项目 README、官网、社区论坛、示例代码仓库等大量外链，确保文档中的引用链接始终处于可访问且版本匹配的状态。

- **内部开发者门户前置治理**：在正式建设内部开发者门户之前，团队可先使用 TerraIndex 对散布在 Wiki、Confluence、钉钉文档中的数百个外链进行清洗、去重与重新分类，为后续门户内容迁移提供干净的数据基础。

## 快速开始

以下指令适用于 Linux / macOS / Windows WSL2 环境，假设系统已安装 Git 与 Python 3.9+。

```bash
# 1. 克隆项目仓库
git clone https://github.com/terraform-index/terraindex.git
cd terraindex

# 2. 安装 Python 依赖（推荐使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# 3. 运行本地校验与索引生成
python scripts/validate.py --source ./data --output ./dist
python scripts/build_index.py --source ./data --output ./dist/index.json

# 4. 启动本地预览服务（可选）
python -m http.server 8000 --directory ./dist
```

执行完成后，可通过浏览器访问 `http://localhost:8000` 查看生成的索引页面。若需要重新生成，重复执行第 3 步即可。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
| :--- | :--- | :--- |
| Python | 3.9, 3.10, 3.11, 3.12 | 核心脚本运行环境；推荐使用 3.11 以获得最佳性能 |
| pip | 21.0 及以上 | Python 包管理工具，用于安装依赖库 |
| Git | 2.25 及以上 | 用于克隆仓库及版本管理；部分校验脚本依赖 git 元信息 |
| PyYAML | 6.0 及以上 | 解析资源节点的 YAML 前置声明，必须安装 |
| requests | 2.28 及以上 | 可选依赖，用于 URL 可达性探测；若未安装，校验脚本将跳过网络检查 |
| pytest | 7.0 及以上 | 仅开发测试时需要，生产环境可不安装 |

## 文档导航

| 层面 | 目录 / 文档 | 回答的问题 |
| :--- | :--- | :--- |
| 入门指南 | `docs/getting-started.md` | 如何安装、初始化数据目录、执行第一次索引构建？ |
| 资源格式规范 | `docs/resource-spec.md` | 每个资源节点 YAML 文件的字段定义、数据类型与必填规则是什么？ |
| 标签体系说明 | `docs/taxonomy.md` | 内置标签分类有哪些？如何自定义或扩展标签？如何映射旧标签？ |
| CLI 命令参考 | `docs/cli-commands.md` | `validate`、`build_index`、`export_csv` 等命令的完整参数列表与示例 |
| 贡献流程 | `CONTRIBUTING.md` | 外部贡献者如何提交新资源节点？代码与资源条目的审核流程有何不同？ |
| 版本发布策略 | `docs/release.md` | 版本号规则、发布周期、向后兼容承诺与迁移指南 |

## 资源列表

### 足球赛事数据分析类

<code>zuqiujinrifenxi.org.cn</code>

<code>zuqiujiebaowang.org.cn</code>

<code>zuqiujishibifen365.org.cn</code>

<code>zuqiujishibifen500.org.cn</code>

<code>zuqiujishibifengf.org.cn</code>

<code>zuqiujishibifenwz.org.cn</code>

<code>zuqiujishibifengw.org.cn</code>

## 项目结构

```
terraindex/
├── data/                                 # 所有资源节点源文件存放目录
│   ├── domains/                          # 按一级领域分类
│   │   ├── realtime/                     # 实时数据处理领域
│   │   │   ├── flink_links.yaml          # Apache Flink 相关资源节点
│   │   │   └── spark_streaming.yaml      # Spark Streaming 资源节点
│   │   ├── frontend/                     # 前端工程化领域
│   │   │   ├── state_management.yaml     # 状态管理方案对比链接
│   │   │   └── build_tools.yaml          # 构建工具与打包工具链
│   │   └── gateway/                      # API 网关与流量治理
│   │       ├── kong_vs_apisix.yaml       # Kong 与 APISIX 对比资源
│   │       └── envoy_proxy.yaml          # Envoy 相关最佳实践
│   ├── tags/                             # 标签索引定义
│   │   ├── primary_tags.yaml             # 一级标签白名单
│   │   └── secondary_mappings.yaml       # 二级标签与同义词映射
│   └── meta/                             # 全局元信息
│       ├── sources.yaml                  # 资源来源机构/作者白名单
│       └── changelog.yaml                # 近期重要变更记录
├── scripts/                              # CLI 工具脚本
│   ├── validate.py                       # 字段校验与 URL 格式检查
│   ├── build_index.py                    # 生成 JSON 索引与统计信息
│   ├── export_csv.py                     # 导出为 CSV 便于 Excel 处理
│   └── utils/                            # 通用工具函数
│       ├── validators.py                 # 自定义校验器
│       └── parsers.py                    # YAML 与 Markdown 解析辅助
├── docs/                                 # 项目文档
│   ├── getting-started.md
│   ├── resource-spec.md
│   ├── taxonomy.md
│   └── cli-commands.md
├── tests/                                # 单元测试与集成测试
│   ├── test_validators.py
│   ├── test_parsers.py
│   └── fixtures/                         # 测试用固定数据集
├── dist/                                 # 构建输出目录（默认，可配置）
│   ├── index.json                        # 主索引文件
│   └── reports/                          # 校验报告与统计快照
├── requirements.txt                      # Python 依赖清单
├── Makefile                              # 常用任务快捷命令
├── LICENSE                               # MIT 许可证
└── README.md                             # 项目说明文档
```

## 贡献指南

1. **提交资源新增请求**：在 `data/domains/` 下选择合适的领域子目录，按照 `docs/resource-spec.md` 中定义的 YAML 格式新建文件，并为每个资源节点填写完整的标题、URL、描述、适用标签与成熟度评估字段。若现有领域目录无法匹配，可在 `data/tags/primary_tags.yaml` 中提议新增一级标签，并同步更新标签映射文件。

2. **本地自检**：在提交 Pull Request 之前，务必在本地运行 `python scripts/validate.py --source ./data` 进行全量校验，确保所有新增或修改的条目通过字段完整性检查与 URL 格式校验。若已安装 requests 库，建议增加 `--check-url` 参数进行网络可达性探测（注意网络环境限制）。

3. **签署开发者原创声明**：所有资源链接必须为公开可访问且不涉及版权限制的内容，提交者需在 PR 描述中明确声明本人已确认每条链接的可用性与内容相关性，且不包含恶意软件或钓鱼站点。

4. **通过 Code Review 与 CI 检查**：项目维护者将审核资源条目的质量、分类准确性与描述客观性。CI 流水线将自动执行校验脚本与单元测试，通过后方可合并至主分支。对于大规模新增或结构调整，建议先提 Issue 进行方案讨论。

5. **更新变更日志**：若提交涉及标签体系变更、目录结构重组或脚本行为调整，需同步更新 `data/meta/changelog.yaml` 并记录变更原因与影响范围，便于后续追溯。

## 常见问题

**Q：TerraIndex 是否会自动爬取或缓存外链内容？**

A：不会。TerraIndex 严格限定为外链索引系统，所有脚本仅读取本地 YAML 文件中明确记录的 URL 字符串，并在校验阶段进行轻量级 HEAD 请求（可选）以验证可达性，但不会下载、解析、存储或缓存任何外部网页内容。项目本身不涉及内容抓取、全文检索或数据持久化外部资源。

**Q：如果某个外链域名在未来无法访问，TerraIndex 如何处理？**

A：TerraIndex 本身不提供自动失效检测与告警能力。团队应定期（建议每月一次）运行 `validate.py --check-url` 对全部资源执行可达性扫描，并根据扫描结果手动更新或移除失效节点。项目鼓励将定期校验纳入团队的日常维护任务，并在内部文档中记录校验结果。

**Q：能否将 TerraIndex 与团队内部的 Confluence 或 Notion 集成？**

A：可以。TerraIndex 的 `dist/index.json` 输出文件提供了完整的结构化资源列表，团队可编写自定义脚本将 JSON 数据转换为 Confluence 表格格式或 Notion API 所需的 payload。项目本身不提供官方集成插件，但持续维护 `export_csv.py` 导出功能，方便进行中间格式转换。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
