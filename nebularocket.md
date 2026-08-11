# OpenResourceHub

OpenResourceHub 是一个面向技术内容创作者与开源项目维护者的外部资源导航与元数据索引工具。项目定位为轻量级、可自托管的资源聚合站点，帮助开发者快速建立结构化的外链资源库，解决项目文档中外部参考链接分散、版本追踪困难、可用性监测缺失等问题。目标用户包括开源项目文档维护者、技术博客作者、在线教育课程设计者以及需要频繁引用外部规范或工具站点的研发团队。

## 功能概览

- **批量链接导入与分类管理**：支持通过 CSV 或 Markdown 列表批量导入外部 URL，并自动按域名、内容类型或自定义标签进行初步归类，便于后续检索与展示。

- **可用性主动监测**：内置定时巡检任务，可对已收录的每个外部链接发起 HTTP 请求，记录响应状态码与响应时间，并在链接失效或证书异常时通过 Webhook 发出告警通知。

- **元数据自动提取**：对于 HTML 页面链接，自动抓取页面标题、描述、关键词及 Open Graph 信息，生成预览卡片所需的结构化数据，减少手动录入工作量。

- **多维度检索与过滤**：支持按链接状态（有效/失效/待检测）、所属分类、创建时间范围以及域名后缀进行组合过滤，便于在大规模资源库中快速定位目标条目。

- **快照与变更追踪**：每次巡检时保存链接响应头的摘要信息，支持查看单个链接的历史可用性趋势图，并可记录页面标题或 robots 协议的变更事件。

- **开放 API 与嵌入组件**：提供 RESTful API 用于外部系统集成，同时提供可嵌入的 React 组件，允许将资源列表直接嵌入其他项目的文档站点或管理后台。

- **权限与协作支持**：基于角色的访问控制，支持多用户协作编辑资源条目，并记录每次修改的操作日志，满足团队内部审计需求。

## 应用场景

- **开源项目文档站的外链管理**：当项目 README 或官方文档中需要引用大量第三方规范、工具链或社区教程时，维护人员可使用 OpenResourceHub 统一管理这些链接，并在文档中通过 API 动态渲染最新的可用链接列表，避免 README 中出现死链。

- **技术课程或实训平台的参考资料库**：在线教育平台可将每门课程涉及的延伸阅读材料、实验环境地址、代码仓库模板等外部资源录入系统，学生端通过统一的资源面板访问，平台方可实时监测所有外部资源的可用性，保障教学体验。

- **企业内部技术导航站**：研发团队可将常用的内部系统地址（如 CI/CD、监控面板、容器仓库、代码评审工具）以及外部依赖（如官方镜像源、SDK 下载页）集中托管于 OpenResourceHub，新员工入职时可快速获取完整的技术生态入口，运维团队可统一监控所有关键外部依赖的连通性。

- **技术博客或周刊的资源聚合页**：独立博主或技术周刊编辑可使用该项目维护每期推荐的优质文章、工具或视频链接，自动生成带预览卡片和状态标识的资源汇总页面，提升读者浏览体验并降低人工检查死链的频率。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，要求已安装 Git、Node.js 18.x 及以上版本以及 npm 或 yarn。

```bash
# 1. 克隆项目仓库
git clone https://github.com/example/OpenResourceHub.git
cd OpenResourceHub

# 2. 安装依赖
npm install

# 3. 配置环境变量（首次运行前需复制示例配置）
cp .env.example .env
# 编辑 .env 文件，至少设置 DATABASE_URL 和 BASE_URL

# 4. 初始化数据库结构
npm run db:migrate

# 5. 启动开发服务器
npm run dev
```

生产环境部署请参考文档导航中的部署指南，建议使用 PM2 或 Docker 方式运行。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，推荐使用官方预编译二进制或 nvm 管理 |
| npm | 9.x 或更高 | 包管理器，也可使用 yarn 1.22+ 替代 |
| PostgreSQL | 14.x 或更高 | 主数据库，用于存储资源条目、用户信息及巡检日志 |
| Redis | 7.x 或更高 | 可选但强烈推荐，用于缓存 API 响应和调度任务状态 |
| 操作系统 | Linux (Ubuntu 20.04+) / macOS 12+ / Windows 11 (WSL2) | 开发与生产环境均以上述系统为主要测试平台 |
| 网络环境 | 可访问公网 | 巡检功能需要对外部域名发起请求，需确保网络出站策略允许 HTTPS/HTTP |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | /docs/getting-started | 如何在一台新机器上完成从零到可访问实例的部署；环境变量每一字段的含义与推荐值 |
| 运维手册 | /docs/operations | 如何配置巡检频率、告警规则及日志轮转；如何备份和恢复数据库；如何迁移至新服务器 |
| 开发贡献 | /docs/contributing | 项目的代码风格规范、Git 提交信息格式、测试运行方法以及 Pull Request 的评审流程 |
| API 参考 | /docs/api-reference | 所有开放接口的请求路径、参数说明、响应结构及错误码释义；API 鉴权方式与限流策略 |

## 资源列表

本项目的设计初衷之一即为高效组织和展示外部资源，以下为初始收录的参考链接分类汇总。所有链接均按用户原始输入原样呈现，不做任何协议补全或域名修改。

### 综合内容导航

<code>oumeishiba.org.cn</code>

<code>tingtingdaohang.org.cn</code>

### 垂直领域资源

<code>yirenguochannv.org.cn</code>

<code>yazhouzhongwenzimutiantang.org.cn</code>

<code>guochanjiqingwangzhan.org.cn</code>

### 专题与工具索引

<code>hanguojiujiu.org.cn</code>

<code>zhongwenzimuzaixianmianfeikan.org.cn</code>

## 项目结构

```
OpenResourceHub/
├── apps/
│   ├── web/                         # 主应用前端界面 (Next.js 页面与路由)
│   ├── api/                         # 后端 API 服务 (Fastify 应用)
│   └── worker/                      # 独立调度与巡检工作进程 (BullMQ 消费者)
├── packages/
│   ├── database/                    # 数据库模型定义与迁移脚本 (Prisma)
│   ├── core/                        # 核心业务逻辑与工具函数 (链接解析、元数据抓取)
│   ├── types/                       # 全局 TypeScript 类型与接口声明
│   └── monitoring/                  # 可用性监测与告警模块 (含 HTTP 巡检器与状态机)
├── configs/
│   ├── eslint/                      # 共享 ESLint 配置文件
│   ├── tsconfig/                    # 各子包继承的基础 TypeScript 配置
│   └── jest/                        # 单元测试与集成测试通用预设
├── scripts/
│   ├── seed.ts                      # 初始数据填充脚本 (用于开发环境)
│   └── healthcheck.sh               # 容器健康检查脚本 (用于 Docker 部署)
├── .env.example                     # 环境变量配置模版 (含数据库连接、Redis 地址、JWT 密钥)
├── docker-compose.yml               # 本地开发所需基础服务编排 (PostgreSQL + Redis)
├── Dockerfile.prod                  # 生产环境多阶段构建镜像定义
└── README.md                        # 项目总体介绍 (即本文档)
```

## 贡献指南

1. **阅读行为准则与贡献公约**：在提交任何代码或文档前，请先阅读项目根目录下的 CODE_OF_CONDUCT.md 文件，并确保您同意其中的协作原则。所有参与人员需遵守开源社区基本礼仪。

2. **从 Issue 或讨论区切入**：优先浏览 GitHub Issues 中标记为 `good first issue` 或 `help wanted` 的条目，或在 Discussions 板块提出新的功能设想。与维护者沟通确认方向后再开始编码，可避免大量返工。

3. **派生仓库并创建功能分支**：将本仓库派生至个人账户，然后克隆派生仓库。新建分支时请使用 `feat/`、`fix/`、`docs/` 或 `chore/` 前缀，后接简要描述，例如 `feat/add-batch-import-csv`。

4. **编写测试与更新文档**：所有新增功能或修复必须包含对应的单元测试或集成测试。同时，需同步更新 docs 目录下的相关文档以及 API 注释。确保 `npm run test` 和 `npm run lint` 全部通过。

5. **发起 Pull Request 并参与评审**：将分支推送至派生仓库后，向主仓库的 `main` 分支发起 Pull Request。请在 PR 描述中关联相关 Issue 编号，并完整填写 PR 模板中的检查清单。至少需要一位项目维护者批准后方可合并。

## 常见问题

**Q: 巡检功能是否会对目标网站造成压力？如何调整巡检频率？**

A: 系统默认采用指数退避策略，单次全量巡检间隔为 6 小时，且并发请求数不超过 5 个，避免对小型站点造成意外负载。您可以在环境变量中通过 `CHECK_INTERVAL_CRON` 和 `MAX_CONCURRENT_CHECKS` 调整频率和并发度。对于内网地址或私有域名，可通过配置 `CHECK_WHITELIST` 与 `CHECK_BLACKLIST` 精确控制巡检范围。

**Q: 如何迁移已有的外链数据（例如从书签 HTML 或 Notion 表格）？**

A: 项目提供了一组数据导入脚本，位于 `scripts/import/` 目录下。目前支持 Netscape 书签格式（HTML）、Notion 导出的 CSV 以及通用 JSON 数组格式。您需要将文件放置于 `data/import/` 目录下，然后运行 `npm run import:csv` 或 `npm run import:html`，并按照交互提示完成字段映射。导入前建议先在测试环境执行一次 dry-run 模式。

**Q: 部署后访问页面出现 502 错误，可能的原因是什么？**

A: 该错误通常与后端服务未正常启动或数据库连接失败有关。请依次检查：1) `.env` 文件中的 `DATABASE_URL` 是否指向正确的 PostgreSQL 实例且密码无误；2) Redis 服务是否已启动且 `REDIS_URL` 配置匹配；3) 使用 `npm run build` 构建时是否有 TypeScript 报错；4) 查看 `logs/` 目录下的应用日志，重点关注 `error` 级别条目。如果使用 Docker 部署，建议先执行 `docker-compose logs api` 查看容器输出。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:21
