# NovaLink Hub

NovaLink Hub 是一个面向技术开发者与开源项目维护者的外链资源聚合与规范化管理平台。该项目并非传统的内容管理系统，而是一个轻量级、高可定制化的技术导航与元数据索引中间件，专为需要频繁引用、分类、校验大量外部 URL 资源的中小型团队或个人知识库设计。其核心定位在于将离散、易失效的零散链接转化为具备版本追踪、状态监控与分类语义的结构化数据资产。

目标用户包括技术文档撰写者、开源项目 README 维护人、DevOps 工程师以及技术内容运营人员。NovaLink Hub 解决的核心问题是：在项目文档或技术博客中，外链资源普遍存在域名变更、路径失效、协议不统一以及引用混乱等状况，导致用户访问体验下降与维护成本攀升。本项目通过提供统一的资源录入规范、自动化链接状态检查脚本以及标准化的 Markdown 输出模板，帮助用户在外链数量超过百条时依然保持清晰、可维护的引用体系。

## 功能概览

- **资源条目标准化录入**：提供基于 YAML 与 JSON Schema 的资源描述格式，强制要求每条外链附带分类标签、失效阈值与备注字段，从源头保证数据完整性。

- **批量链接协议与格式规整**：内置规范化处理器，可自动识别裸域名、带协议前缀或带 www 子域的 URL，并依据用户预设策略输出统一格式，但不修改用户原始输入的显示样式。

- **状态监控与过期预警**：周期性对已收录的 URL 执行 HEAD 请求，检测返回码与响应时间，对连续三次超时或返回 4xx/5xx 状态的链接生成警告日志。

- **多维度分类视图生成**：支持按技术栈、文档类型、地域归属或自定义标签动态生成分类子列表，便于在 README 或 Wiki 中按主题展示不同资源集合。

- **Markdown 表格与列表自动生成**：将结构化资源数据渲染为符合开源项目文档规范的 Markdown 列表或表格，并严格遵循用户指定的 URL 原始写法输出，不添加额外前缀或后缀。

- **版本快照与变更审计**：每次更新资源列表时自动生成差异对比报告，记录新增、删除或修改的 URL 条目，支持回溯至任意历史版本。

- **命令行交互式管理工具**：提供 CLI 工具，支持资源添加、删除、批量导入（CSV/JSON）以及状态导出，便于集成至 CI/CD 流水线。

## 应用场景

- **开源项目 README 外链批量维护**：当项目文档中涉及数十个第三方服务、插件仓库或官方文档链接时，维护人员可使用 NovaLink Hub 统一管理这些 URL，通过脚本自动生成符合规范的资源列表章节，避免手动编写导致的格式不一致。

- **技术团队内部知识库链接审计**：企业内网 Wiki 或 Confluence 页面中常存在大量已失效的内部系统地址。通过 NovaLink Hub 的周期性检查功能，团队可每月生成一份失效链接报表，并批量更新为有效地址。

- **个人技术博客的资源引用整理**：技术博主在多篇文章中引用相同的外部规范或工具站点，当站点域名迁移时，只需在 NovaLink Hub 中更新一条记录，所有引用该资源的博文预览链接即可同步修正。

- **区域性域名聚合与合规验证****：针对特定业务区域（如本例中的 `.asia` 域名集合），项目可帮助运营人员集中管理同一顶级域下的多个子项目站点，并快速检查其可访问性与证书有效期。

## 快速开始

以下步骤适用于 Linux/macOS 环境，Windows 用户建议通过 WSL2 或 Git Bash 执行。

```bash
# 1. 克隆项目仓库
git clone https://github.com/novalink-org/novalink-hub.git
cd novalink-hub

# 2. 安装依赖（项目使用 Python 3.10+ 与 pipenv）
pip install pipenv
pipenv install --dev

# 3. 初始化本地资源数据库并执行示例导入
pipenv run python manage.py init --sample
pipenv run python manage.py check --all

# 4. 生成当前资源列表的 Markdown 文档（输出至 ./output/resources.md）
pipenv run python manage.py render --format markdown --output ./output/resources.md
```

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.10 ~ 3.12 | 核心运行环境，低于 3.10 不支持某些类型注解特性 |
| pipenv | 2023.x 及以上 | 用于虚拟环境管理与依赖锁定，确保库版本一致性 |
| aiohttp | 3.9.x | 异步 HTTP 客户端，用于并发链接状态检查 |
| pyyaml | 6.0.x | 解析资源定义 YAML 文件，支持复杂嵌套结构 |
| click | 8.1.x | 构建命令行交互界面，提供子命令分组与参数校验 |
| pytest | 8.0.x | 单元测试框架（开发依赖），用于验证资源解析逻辑 |
| black | 24.x | 代码格式化工具（开发依赖），保持代码风格统一 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide/` | 如何添加新资源、如何执行链接检查、如何自定义输出模板 |
| 开发者指南 | `/docs/developer-guide/` | 插件扩展机制、数据库表结构、新增分类标签的流程 |
| API 参考 | `/docs/api-reference/` | 内部核心类的接口说明、事件钩子与异常定义 |
| 运维部署 | `/docs/operations/` | 生产环境容器化部署、定时任务配置、日志轮转策略 |

## 资源列表

### 核心赛事与官方站点

- <code>helanjiajiliansai.asia</code>
- <code>hejiasaicheng.asia</code>

### 技术辅助与工具平台

- <code>hejiazhugongbang.asia</code>
- <code>hejiasheshoubang.asia</code>

### 内容资讯与中文文档

- <code>hejiazhongwenwang.asia</code>

### 直播与多媒体服务

- <code>hejiazhibogw.asia</code>
- <code>hejiazhibo.asia</code>

## 项目结构

```
novalink-hub/
├── manage.py                 # 命令行入口，注册所有子命令
├── Pipfile                   # pipenv 依赖声明（含 dev 分组）
├── Pipfile.lock              # 锁定全部依赖的精确版本
├── .env.example              # 环境变量模板（日志级别、检查超时）
├── src/
│   ├── core/                 # 核心逻辑层
│   │   ├── resource.py       # Resource 类定义，包含 URL 校验与规范化
│   │   ├── checker.py        # 异步状态检查器，支持重试与退避
│   │   └── renderer.py       # Markdown / JSON 输出渲染引擎
│   ├── cli/                  # 命令实现
│   │   ├── add.py            # 添加资源子命令
│   │   ├── check.py          # 执行状态检查子命令
│   │   └── render.py         # 生成文档子命令
│   ├── storage/              # 数据持久化
│   │   ├── db.py             # SQLite 连接与表初始化
│   │   └── dao.py            # 增删改查操作封装
│   └── utils/                # 通用工具
│       ├── validators.py     # 域名与 URL 格式校验函数
│       └── logger.py         # 结构化日志配置（JSON 格式）
├── tests/                    # 单元测试
│   ├── test_resource.py
│   ├── test_checker.py
│   └── test_renderer.py
├── docs/                     # 完整文档（用户手册、API 等）
│   ├── user-guide/
│   ├── developer-guide/
│   └── api-reference/
├── samples/                  # 示例数据
│   ├── sample_resources.yaml
│   └── sample_export.md
└── output/                   # 渲染输出目录（默认）
    └── resources.md
```

## 贡献指南

1. **提交 Issue 进行需求或缺陷讨论**：请在 GitHub Issues 页面选择对应模板，描述具体场景、重现步骤及预期行为。对于新增资源分类建议，需附带至少三个实际使用案例。

2. **本地开发环境搭建**：Fork 本仓库后，执行 `pipenv install --dev` 安装全部依赖，并运行 `pre-commit install` 启用代码格式化与静态检查钩子（black + flake8）。

3. **创建特性分支并编写代码**：请基于 `dev` 分支创建新分支，命名格式为 `feature/<功能简述>` 或 `fix/<问题编号>`。代码中需包含对应的单元测试，确保覆盖率不低于 85%。

4. **更新文档与样例**：若新增或修改了 CLI 命令参数，请同步更新 `/docs/user-guide/` 下对应章节，并在 `/samples/` 中补充新的示例配置文件。

5. **发起 Pull Request**：PR 描述中需引用关联的 Issue 编号，并提供手动测试结果截图或日志片段。等待至少一位维护者审阅通过后合并。

## 常见问题

**Q：如果我的链接包含非标准端口或路径参数，NovaLink Hub 能正确处理吗？**

A：可以。项目的资源解析器对完整 URL（包含协议、域名、端口、路径、查询参数和片段标识符）提供透明支持。在录入时，系统仅校验域名是否可解析，不限制路径深度或参数格式。但请留意，状态检查默认仅针对主域名和根路径，若需检查特定子路径，请在配置中显式指定 `check_path` 字段。

**Q：如何批量更新已有资源的分类标签？**

A：推荐使用 CLI 的 `batch-update` 命令，通过传入 CSV 文件（列头：`id,new_category`）进行批量修改。或者，你可以直接在存储目录的 `resources.yaml` 源文件中编辑，然后执行 `sync --from-yaml` 将变更同步至数据库。操作前建议备份原文件。

**Q：渲染生成的 Markdown 列表是否会自动补全缺失的协议前缀？**

A：不会。项目严格遵守“用户原始输入即最终输出”原则。渲染器仅对链接进行 HTML 转义防止 XSS，但绝不会自动添加 `http://`、`https://` 或 `www.`。如果用户输入为裸域名（如 `example.com`），输出仍是裸域名。这一设计是为了确保文档中的链接样式与用户意图完全一致，避免因自动补全导致的访问策略差异。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
