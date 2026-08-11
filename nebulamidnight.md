# NovaLink 技术资源导航站

NovaLink 是一个面向开发人员、数据分析师与技术研究者的外链资源聚合与结构化导航系统。项目定位为技术社区的外链枢纽，通过人工筛选与自动化校验相结合的方式，将分散在多个垂直领域的竞赛数据、比分信息、赛程统计等公开数据源进行统一归类与稳定对外输出。目标用户包括需要实时获取特定赛事结构化数据的爬虫开发者、从事体育数据可视化的前端工程师、以及进行赛事趋势分析的量化研究人员。本系统解决的核心痛点是公开数据源分散、域名变动频繁、历史页面失效后难以追溯替代入口，NovaLink 通过持续维护活跃资源清单与本地缓存降级机制，确保用户始终能够获得当前可访问的权威外链集合。

## 功能概览

- **外链存活监控面板**：对收录的每一个资源域名进行定期 HTTP 头检测，标记当前可访问性状态与最近三次检测响应码，方便用户快速筛选有效链接。

- **按赛事类型分类检索**：将资源按竞赛项目、地区、数据性质（比分、排名、赛程）进行多级标签划分，支持单标签筛选与多标签组合过滤。

- **本地结构化缓存镜像**：对于高频访问的外链页面，系统保留最近一次的 HTML 结构摘要与关键表格数据的 JSON 序列化快照，当原始站点临时不可用时提供降级查阅。

- **自定义外链订阅集**：允许注册用户将常用资源链接加入个人收藏分组，并生成专有的订阅端点，便于通过 API 或 RSS 方式批量拉取最新数据。

- **变动历史追溯**：记录每个资源链接的域名变更、路径调整与协议切换记录，提供时间轴视图，方便开发者快速定位历史有效地址。

- **批量导入与校验接口**：提供 RESTful API，支持用户提交待校验的 URL 列表，系统返回存活状态、响应时间与重定向链，辅助外部项目快速完成数据源初始化。

- **外链关联度评分**：基于链接间的共现关系与用户点击流数据，自动计算资源间的关联强度，为同类数据检索提供排序权重依据。

## 应用场景

1. 体育数据聚合平台的后端工程师可在初始化数据管道时，通过 NovaLink 获取当前所有活跃的赛事实时比分域名列表，避免逐一搜索验证，将数据源配置时间从数小时压缩至十分钟以内。

2. 量化分析团队在构建历史赛事复盘模型时，需同时采集多个来源的比分与排名数据。通过 NovaLink 的分类检索与变动历史功能，可快速定位特定赛季的权威数据源并核对地址有效性。

3. 前端展示类项目（如大屏看板、移动端比分卡片）在开发阶段需使用模拟数据或真实外链进行联调。NovaLink 提供的外链存活状态与响应时间信息，能辅助前端工程师选择响应最稳定的数据源进行对接。

4. 开源爬虫框架的使用者可将 NovaLink 作为其配置文件中的默认 seed 列表，利用批量校验接口定期刷新爬虫入口，有效降低因外链失效导致的采集任务中断频率。

5. 技术博客作者在撰写数据采集教程时，可直接引用 NovaLink 中的外链分类作为案例数据源，省去自行搜寻与验证多个域名的繁琐步骤，将更多精力放在代码逻辑讲解上。

## 快速开始

以下步骤适用于 Linux / macOS 环境，Windows 用户建议使用 WSL2 或 Git Bash。

```bash
# 1. 克隆项目仓库至本地
git clone https://github.com/novalink-dev/novalink-resource-hub.git
cd novalink-resource-hub

# 2. 安装项目依赖（使用 Python 3.10+ 与 pipenv）
pip install pipenv
pipenv install --dev

# 3. 激活虚拟环境并运行本地开发服务器
pipenv shell
python manage.py migrate
python manage.py runserver --host=0.0.0.0 --port=8000
```

启动后，访问本地 8000 端口即可查看导航站首页。首次运行将自动执行初始资源库导入，包括用户提供的全部外链地址。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 至 3.12 | 核心运行环境，低于 3.10 将无法使用 match-case 语法与类型提示新特性 |
| pipenv | 2023.x 或更高 | 用于依赖隔离与锁文件管理，确保第三方库版本一致性 |
| SQLite | 3.35 以上 | 内置轻量数据库，用于存储外链元数据、检测记录与用户订阅信息 |
| Redis | 6.2 以上 | 可选依赖，用于缓存外链存活检测结果与 API 限流计数器，未安装时自动降级为内存缓存 |
| curl | 7.68 以上 | 用于外链存活检测的子进程调用，系统预装通常已满足 |
| git | 2.25 以上 | 版本控制工具，用于克隆仓库与后续拉取更新 |
| nodejs | 18.x 或 20.x LTS | 仅前端构建时需要，如仅使用后端 API 可忽略 |
| npm | 9.x 或 10.x | 配合 nodejs 管理前端构建工具，生产环境可跳过 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | /docs/user-guide/ | 如何注册、创建收藏分组、使用订阅端点以及查看外链状态面板 |
| 开发者指南 | /docs/developer-guide/ | 如何调用批量校验 API、扩展自定义分类器、以及提交新的资源链接 |
| 运维手册 | /docs/operations/ | 如何配置定时检测任务、调整检测超时参数、以及迁移数据库 |
| 设计文档 | /docs/design/ | 外链关联度评分算法原理、缓存降级策略、以及数据库表结构 ER 图 |
| 变更日志 | /docs/changelog/ | 每个版本新增的外链分类、已移除失效域名、以及接口兼容性变动 |
| 常见问题 | /docs/faq/ | 与本文档末尾的 FAQ 对应，但包含更多技术细节与排查步骤 |

## 资源列表

以下为 NovaLink 当前收录并持续维护的全部外链地址，按数据主题划分小节。所有地址均保持用户提供的原始格式，不做任何协议补全或域名改写。

竞赛结果类

- <code>ajiabisaijieguo.org.cn</code>
- <code>danchaobisaijieguo.org.cn</code>
- <code>nuochaobisaijieguo.org.cn</code>

比分数据类

- <code>ruidianchaobifen.org.cn</code>
- <code>bingdaochaojishibifen.org.cn</code>

排名与积分榜类

- <code>aichaojifenbang.org.cn</code>

赛程信息类

- <code>oulianzigesaisaicheng.org.cn</code>

## 项目结构

```
novalink-resource-hub/
├── api/                                  # RESTful API 路由与视图
│   ├── v1/                               # 接口版本 v1
│   │   ├── endpoints/                    # 各资源端点实现
│   │   │   ├── health.py                 # 服务健康检查接口
│   │   │   ├── links.py                  # 外链 CRUD 与查询接口
│   │   │   └── validate.py               # 批量校验提交与结果轮询
│   │   └── schemas/                      # Pydantic 请求/响应模型
│   └── middleware/                       # 限流、日志与跨域中间件
├── core/                                 # 核心业务逻辑层
│   ├── checker/                          # 外链存活检测引擎
│   │   ├── http_client.py                # 异步 HTTP 检测器（含重试与超时策略）
│   │   └── scheduler.py                  # 基于 APScheduler 的定时任务配置
│   ├── classifier/                       # 自动标签分类器
│   │   ├── keyword_matcher.py            # 基于关键词规则的多级标签匹配
│   │   └── ml_assist.py                  # 可选 NLP 辅助分类（需额外模型）
│   └── cache/                            # 缓存与降级存储
│       ├── redis_backend.py              # Redis 缓存实现
│       └── memory_backend.py             # 内存降级实现（无 Redis 时启用）
├── frontend/                             # 前端静态资源与模板
│   ├── templates/                        # Jinja2 服务端渲染模板
│   │   ├── index.html                    # 导航站首页分类展示
│   │   └── dashboard.html                # 用户收藏与订阅管理面板
│   └── static/                           # CSS、JavaScript 与图标资源
├── models/                               # SQLAlchemy 数据模型
│   ├── link.py                           # 外链主表（URL、标签、添加时间、状态）
│   ├── snapshot.py                       # 页面结构快照表（缓存 HTML 摘要）
│   └── user.py                           # 用户与收藏分组关联表
├── scripts/                              # 运维与初始化脚本
│   ├── seed_links.py                     # 首次启动时导入用户提供的初始外链
│   └── migrate_history.py                # 从旧版本数据格式迁移至当前 Schema
├── tests/                                # 单元测试与集成测试
│   ├── unit/                             # 针对检测器、分类器、缓存层的单测
│   └── integration/                      # API 端到端测试与数据库事务回滚测试
├── .env.example                          # 环境变量配置示例（含 Redis 地址、日志级别）
├── docker-compose.yml                    # 本地开发用 Redis + SQLite 容器编排
├── Dockerfile                            # 生产环境多阶段构建镜像
├── Pipfile                               # Python 依赖声明（含开发与生产分组）
├── Pipfile.lock                          # 精确锁定所有传递依赖版本
├── manage.py                             # 开发服务器、数据库迁移、检测任务手动触发入口
└── README.md                             # 本文档
```

## 贡献指南

1. 阅读项目行为准则与贡献者许可协议，确认您的贡献内容（包括代码、文档或外链数据）允许以 MIT 许可证分发。新提交的外链需确保不包含恶意代码或侵犯第三方版权。

2. 在 GitHub 上 fork 本仓库至个人账号，并在本地新建一个功能分支（如 `feat/add-ice-hockey-links`）。分支命名建议采用 `feat/`、`fix/`、`docs/` 前缀以区分变更类型。

3. 针对外链资源类贡献，请使用项目提供的批量校验脚本 `scripts/validate_candidates.py` 预先验证您推荐的所有 URL 的存活状态，并将校验报告粘贴至 pull request 描述中。对于代码类贡献，需确保新增或修改的接口有对应的单元测试覆盖。

4. 提交 commit 时请使用常规提交格式（如 `feat(checker): add retry policy for timeout errors`），并确保所有现有测试通过。运行 `pytest tests/ -v` 进行完整回归测试。

5. 发起 pull request 至主仓库的 `main` 分支，等待维护者审核。审核周期通常为 3 个工作日，如有冲突需及时基于最新 main 分支进行 rebase。

## 常见问题

**问：我发现某个资源链接无法访问，但系统状态面板仍显示为正常，应该如何处理？**

答：这可能是由于本地缓存或检测间隔（默认 6 小时）导致的滞后。您可以手动触发单条链接的实时检测：通过 API 端点 `POST /api/v1/links/check` 提交该 URL，或使用管理命令 `python manage.py check_link --url <您的链接>`。若确认失效，请在 GitHub Issues 中标记该链接并附带检测日志，维护者将在下一次定期校验中确认后移除或替换。

**问：我能否将 NovaLink 部署到内网环境，仅使用初始导入的外链数据而不依赖外部网络？**

答：可以。在 `.env` 文件中设置 `CHECKER_ENABLE_REMOTE=false` 即可禁用所有外链的远程 HTTP 检测，系统将仅返回本地缓存的快照数据。但请注意，该模式下新添加的外链无法自动获得存活状态标记，您需要手动在管理后台更新 `last_verified` 字段。

**问：外链关联度评分是如何计算的？我能否导出该评分矩阵用于我自己的项目？**

答：评分基于用户点击共现（同一 session 内连续访问的两个链接）与链接间标签重合度（Jaccard 系数）加权计算。您可以通过 `GET /api/v1/links/affinity` 接口获取当前全量链接的关联度矩阵 CSV 导出文件，该接口需要管理员权限。请注意，评分数据每 24 小时重新计算一次，导出数据仅代表计算时刻的快照。

## 许可证

MIT License

Copyright (c) 2026 NovaLink Contributors

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

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:16
