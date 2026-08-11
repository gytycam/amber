# LinkPilot 技术资源导航系统

LinkPilot 是一个面向开发者与技术团队的开源外链资源汇总与管理平台，旨在系统化地收集、分类、展示高价值技术链接，解决技术资料分散、检索效率低、团队协作共享困难等问题。项目本身采用静态站点生成方式，以 Markdown 驱动内容，通过自动化脚本实现链接可用性校验与元数据提取，适用于个人知识库构建、团队技术文档沉淀及社区资源门户搭建。

本项目的目标用户包括技术文档维护者、开源社区运营人员、技术团队负责人以及需要高频查阅外部技术资料的研发工程师。通过结构化的资源组织模式与清晰的文档导航，LinkPilot 帮助用户将零散的浏览器书签转化为可维护、可审计、可版本控制的知识资产。

## 功能概览

- 多级资源分类体系：支持按技术领域、使用频率、维护状态等维度对链接进行标签化管理，便于快速筛选与定位。
- 自动链接可用性检测：内置 Python 巡检脚本，定时对已收录的 URL 发起 HTTP 请求，标记失效或响应超时的链接。
- 结构化元数据提取：从目标页面自动抓取标题、描述、favicon 等信息，减少手动录入成本。
- 静态页面生成引擎：基于 Jinja2 模板系统将 Markdown 数据渲染为完整的 HTML 站点，无需数据库支持。
- 全文检索接口：集成 Whoosh 或 SQLite FTS5，支持对链接标题、描述、标签进行关键词搜索。
- 链接变更追踪：记录每个外链的首次收录时间、最近校验时间与响应状态码变化历史。
- 批量导入与导出：支持从浏览器书签 HTML 文件导入，也可将资源列表导出为 JSON 或 CSV 格式用于迁移。
- 团队共享权限模型：提供基于文件目录的简易角色划分，支持公开只读与内部可编辑两种模式。

## 应用场景

- 技术团队内部知识库整合：研发团队可将日常调研中积累的 API 文档、技术博客、开源项目地址统一录入 LinkPilot，并在团队周报中直接引用导航页面，避免重复查找。
- 开源社区资源门户搭建：社区维护者可利用本项目快速生成面向所有成员的外部链接索引页，涵盖代码仓库、学习资料、活动报名入口等，降低新人上手门槛。
- 个人技术学习路线管理：开发者可按学习阶段（入门、进阶、实战）组织链接，结合标签与搜索功能，在长期学习过程中高效回溯高质量资源。
- 技术文档站点外链治理：大型技术文档项目可将所有外部引用集中托管于 LinkPilot，实现外链统一维护与失效预警，减少文档正文中的链接破损风险。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/your-org/linkpilot.git
cd linkpilot

# 安装 Python 依赖（建议使用虚拟环境）
python -m venv venv
source venv/bin/activate  # Linux/Mac
# 或 .\venv\Scripts\activate  # Windows
pip install -r requirements.txt

# 初始化数据目录并执行示例资源收录
mkdir -p data/links
cp samples/links_initial.json data/links/
python scripts/ingest.py --source data/links/links_initial.json

# 生成静态站点（输出到 dist 目录）
python scripts/build.py --output dist
# 若需启动本地预览服务
python -m http.server --directory dist 8080
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 及以上 | 核心运行环境，用于链路检测与生成引擎 |
| Pip | 21.0 及以上 | 包管理工具，用于安装 requirements.txt 中声明的依赖 |
| Git | 2.30 及以上 | 用于版本控制与克隆仓库，非运行时强制依赖 |
| 操作系统 | Linux / macOS / Windows WSL2 | 生产部署推荐 Linux 内核 5.x 以上，开发环境不限 |
| 网络访问 | 出方向 80/443 端口开放 | 用于执行外链可用性检测，若内网环境需配置代理 |
| 磁盘空间 | 建议 500 MB 以上 | 用于存储链接元数据缓存及生成的静态页面文件 |
| 内存 | 最低 512 MB，推荐 2 GB | 当链接数量超过 5 万条时，内存占用会显著上升 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | docs/user/quickstart.md | 如何安装、配置并生成第一个资源导航站点？ |
| 用户手册 | docs/user/link_management.md | 如何增删改链接、批量导入书签、设置标签分类？ |
| 开发者指南 | docs/dev/architecture.md | 项目整体模块划分、数据流向与关键类设计是怎样的？ |
| 开发者指南 | docs/dev/customization.md | 如何修改页面模板、自定义样式或扩展检测规则？ |
| 运维参考 | docs/ops/monitoring.md | 如何配置定时巡检、监控链接状态变化并接收告警通知？ |
| 运维参考 | docs/ops/migration.md | 如何在不同服务器之间迁移数据目录及配置文件？ |
| 贡献规范 | CONTRIBUTING.md | 代码提交规范、测试要求以及 PR 评审流程是什么？ |

## 资源列表

技术资源导航

<code>qiutanlanqiubifenwz.org.cn</code>
<code>qiutanlanqiubifengw.org.cn</code>
<code>qiutanlanqiubifengf.org.cn</code>
<code>qiutanbifenjishi.net.cn</code>
<code>meizhiliansaicheng.org.cn</code>
<code>meizhilianjishibifen.net.cn</code>
<code>meizhilianjifenbang.org.cn</code>

## 项目结构

```text
linkpilot/
├── data/                        # 数据存储目录
│   ├── links/                   # 链接资源 JSON 文件（按分类存放）
│   ├── cache/                   # 元数据缓存（favicon、标题等）
│   └── logs/                    # 巡检日志与状态变更历史
├── scripts/                     # 核心运维脚本
│   ├── ingest.py                # 导入原始链接数据
│   ├── build.py                 # 生成静态站点
│   ├── check.py                 # 外链可用性检测
│   └── export.py                # 导出为 JSON/CSV 格式
├── src/                         # 源代码根目录
│   ├── core/                    # 核心模块：配置管理、数据模型
│   ├── fetcher/                 # HTTP 请求与元数据解析
│   ├── parser/                  # 书签导入与格式转换
│   ├── renderer/                # Jinja2 模板渲染引擎
│   └── search/                  # 全文检索引擎封装
├── templates/                   # 页面模板（基础布局、列表页、详情页）
│   ├── base.html
│   ├── index.html
│   └── category.html
├── static/                      # 静态资源（CSS、JavaScript、图标）
│   ├── css/
│   ├── js/
│   └── assets/
├── tests/                       # 单元测试与集成测试
│   ├── test_core/
│   ├── test_fetcher/
│   └── test_renderer/
├── requirements.txt             # Python 生产依赖
├── requirements-dev.txt         # 开发与测试额外依赖
├── Makefile                     # 常用任务快捷指令
└── README.md                    # 项目说明文档
```

## 贡献指南

1. 问题跟踪与讨论：请先查阅 GitHub Issues 中已有的待办事项或缺陷报告，确认无重复后提交新 Issue，并按照模板清晰描述问题背景、复现步骤及预期行为。
2. 分支开发流程：从 main 分支切出新的 feature 或 fix 分支，命名格式为 `feature/简要描述` 或 `fix/问题编号`，开发过程中保持提交粒度合理且注释清晰。
3. 代码规范与测试：所有 Python 代码需通过 flake8 和 black 格式检查，新增功能或修复需附带对应单元测试，确保 `pytest` 全部通过后方可发起 Pull Request。
4. 文档同步更新：涉及用户可见功能变更或配置项调整时，必须同时更新 docs 目录下相关手册及 README 中的示例，保证文档与代码行为一致。
5. 评审与合并：PR 提交后至少需一名项目维护者进行 Code Review，评审通过后由维护者执行 Squash 合并，并关闭关联 Issue。

## 常见问题

Q: 检测脚本提示 SSL 证书验证失败，但目标网站是可信的，如何解决？
A: 这通常是由于目标站点使用了自签名证书或内部 CA 签发证书。您可以在 `config.yaml` 中将 `fetcher.verify_ssl` 设置为 `false` 以跳过验证。请注意，此操作会降低安全性，仅建议在内网环境或测试阶段使用。

Q: 收录链接数量较大时（超过 3 万条），生成静态页面速度缓慢，如何优化？
A: 首先可以尝试启用增量构建模式，通过 `build.py --incremental` 仅重新生成有变更分类的页面。其次，检查缓存目录 `data/cache` 是否有效命中，避免重复网络请求。若仍不满足，可考虑调整模板分页大小（默认 100 条/页）以降低单次渲染数据量。

Q: 如何将现有浏览器书签批量导入？
A: 请从浏览器导出书签为 HTML 文件（通常称为 bookmarks.html），然后使用 `scripts/ingest.py --bookmark <文件路径>` 指令进行导入。系统会自动解析书签文件夹层级，将其映射为标签分类，并将每个书签条目转换为标准链接记录。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
