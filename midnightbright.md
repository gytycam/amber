# ChronoLink Navigator

ChronoLink Navigator 是一个面向技术决策者、数据分析师与开源项目维护者的结构化外链资源聚合与导航系统。该项目不提供内容存储或代理转发，而是基于人工筛选与自动健康检查相结合的方式，维护一份高可用性、低失效性的外部信息源索引。其核心定位为“链接的元目录”，帮助用户从分散的垂直领域站点中快速定位具备时效性与领域权威性的数据页面，从而降低信息检索过程中的认知负载与时间成本。

目标用户包括但不限于：需要定期查阅特定领域动态报表的产品经理、进行竞品情报采集的市场研究员、搭建数据看板的数据工程师，以及希望将外部参考源系统化嵌入内部文档体系的技术写作者。ChronoLink Navigator 不替代搜索引擎，也不构建新的信息孤岛，而是通过严格的链接分类体系与存活监测机制，为用户提供一份可信任的“起点清单”。

## 功能概览

- **多维度分类索引**：所有收录链接按内容领域、更新频率与来源性质划分至独立章节，支持快速过滤与批量导出。

- **链接健康状态轮询**：内置定时任务，对每一条收录 URL 执行 HTTP 状态码检查与响应时间记录，异常链接自动标记并移入待审核队列。

- **元信息自动补全**：对通过健康检查的链接，自动抓取页面标题、描述关键词与最后修改时间，丰富索引视图的展示维度。

- **只读只存不代理**：系统仅存储链接地址及其元数据，不缓存页面内容，不修改原始资源，完全遵守外部站点的 robots 协议。

- **历史快照对比**：记录每次健康检查的响应状态与页面标题变化，生成趋势图表，辅助判断外部资源的内容稳定性。

- **标签体系与全文检索**：支持为链接自定义业务标签，并提供基于标题、描述、标签的轻量级检索接口，便于与内部 wiki 或监控系统集成。

- **导出与订阅能力**：支持将筛选结果导出为 JSON、CSV 或 OPML 格式，并提供只读的 RSS 订阅流，方便下游系统消费。

## 应用场景

- **行业动态日报自动整理**：市场团队可配置 ChronoLink Navigator 定时拉取指定分类下的所有链接状态，将更新活跃的页面标题与摘要聚合为日报邮件，替代每日手动翻阅数十个书签的重复劳动。

- **技术文档外部引用源管理**：开源项目维护者在编写架构说明或性能对比报告时，需要引用外部测试数据或赛事统计结果。使用 ChronoLink Navigator 可统一管理这些引用链接，并在链接失效时自动告警，避免文档中出现死链。

- **数据中台的源端可用性监控**：数据采集任务依赖多个外部数据源页面。将源端 URL 录入系统后，可借助健康检查结果作为采集任务的前置触发条件，仅在源端可达时启动管道，减少无效重试与日志噪音。

- **竞品信息聚合看板搭建**：分析师可将不同平台的排行榜、预测分析、实时比分等页面纳入同一看板框架，通过 ChronoLink Navigator 提供的只读 API 将链接状态与标题更新推送至 Grafana 或 Metabase，实现可视化监控。

## 快速开始

以下步骤适用于 Linux / macOS 环境，假设已安装 Git 与 Node.js 18+。

```bash
# 1. 克隆仓库
git clone https://github.com/chronolink/navigator.git
cd navigator

# 2. 安装依赖
npm install --production=false

# 3. 初始化配置（复制示例环境变量文件）
cp .env.example .env

# 4. 执行数据库迁移
npm run migrate

# 5. 启动开发服务器
npm run dev
```

访问控制台输出中显示的本地地址（通常为 http://localhost:3000）即可进入索引管理界面。生产环境部署请参考 `docs/deployment.md` 中的 Docker 与反向代理配置说明。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，需支持 ES2022 特性 |
| npm | 9.x 或更高 | 包管理器，用于安装依赖与执行脚本 |
| PostgreSQL | 14.x 或更高 | 主数据库，存储链接元数据、检查记录与标签 |
| Redis | 7.x 或更高 | 缓存会话与健康检查结果，降低数据库查询压力 |
| Git | 2.30 或更高 | 版本控制，用于克隆仓库与获取提交元信息 |
| 系统时区 | UTC+8 或任意 | 定时任务基于系统时区触发，建议显式设置 TZ 环境变量 |
| 磁盘空间 | 至少 1GB 可用 | 用于存储日志文件与 SQLite 临时缓存（若启用） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `docs/user-guide/` | 如何添加链接、编辑分类、配置健康检查频率与查看统计报表 |
| 运维手册 | `docs/operations/` | 如何备份数据库、迁移版本、扩缩容 worker 进程与处理告警 |
| API 参考 | `docs/api/` | 如何通过 RESTful 接口查询链接状态、批量导入导出与集成 Webhook |
| 贡献者指南 | `docs/contributing/` | 代码规范、提交信息格式、测试要求与 PR 审核流程 |
| 架构设计 | `docs/architecture/` | 系统模块划分、数据流方向、扩展点设计以及健康检查算法的权衡说明 |
| 常见问题 | `docs/faq.md` | 收录链接的标准是什么？检查间隔如何调整？如何迁移已有书签？ |

## 资源列表

以下为 ChronoLink Navigator 初始索引库中收录的全部外部资源链接，按主题域分组。所有链接均以原始形式记录，未做协议补全或域名规范化处理。

竞技赛事数据域

<code>ribenzhiyezuqiujiajiliansai.asia</code>

<code>qiutanzuixinyuce.asia</code>

<code>qiutanzuixinfenxi.asia</code>

实时比分与统计域

<code>qiutanjishibifen.asia</code>

<code>qiutanzuqiufenxi.asia</code>

<code>qiutanwanchangbifen.asia</code>

赛事前瞻与预测域

<code>qiutansaishiqianzhan.org.cn</code>

## 项目结构

```
navigator/
├── src/
│   ├── core/                     # 核心模块：链接索引、分类树、标签引擎
│   │   ├── indexer.js            # 新增/更新链接的索引逻辑，含去重与规范校验
│   │   ├── classifier.js         # 基于规则与关键词的自动分类建议
│   │   └── tagEngine.js          # 标签聚合与权重计算
│   ├── health/                   # 健康检查子系统
│   │   ├── checker.js            # 并发 HTTP 探针，支持重试与超时策略
│   │   ├── scheduler.js          # 基于 cron 表达式的轮询调度器
│   │   └── reporter.js           # 状态变更通知与摘要生成
│   ├── api/                      # RESTful 接口层
│   │   ├── v1/                   # 版本化路由：链接管理、查询、导出
│   │   └── middleware/           # 鉴权、限流、日志中间件
│   ├── db/                       # 数据库访问层
│   │   ├── migrations/           # PostgreSQL 迁移脚本（按时间戳命名）
│   │   ├── models/               # 链接、检查记录、标签等数据模型
│   │   └── client.js             # 连接池与事务封装
│   ├── services/                 # 外部集成与后台任务
│   │   ├── rssFeed.js            # RSS 订阅流生成器
│   │   ├── opmlExport.js         # OPML 格式导出器
│   │   └── webhookDispatcher.js  # 状态变更回调分发
│   └── utils/                    # 工具函数：日志、配置解析、URL 规范辅助
│       ├── logger.js             # 结构化日志（JSON 格式，可接入 ELK）
│       ├── config.js             # 环境变量与默认值合并
│       └── urlHelper.js          # 域名提取、协议补全提示、非法字符过滤
├── tests/                        # 单元测试与集成测试
│   ├── unit/                     # 核心模块与工具函数覆盖率测试
│   └── integration/              # API 端到端测试与数据库回滚测试
├── docs/                         # 全部文档源文件（Markdown）
│   ├── user-guide/
│   ├── operations/
│   ├── api/
│   ├── contributing/
│   ├── architecture/
│   └── faq.md
├── scripts/                      # 运维辅助脚本
│   ├── seed.js                   # 初始化索引库示例数据
│   ├── backup.js                 # 数据库与配置备份打包
│   └── migrate-legacy.js         # 从旧版书签格式导入
├── .env.example                  # 环境变量模板（含数据库连接串、调度间隔）
├── .eslintrc.json                # 代码风格与质量检查规则
├── .prettierrc                   # 统一格式化配置
├── package.json                  # npm 依赖与脚本入口
├── Dockerfile                    # 多阶段构建镜像定义
├── docker-compose.yml            # 本地开发依赖栈（PostgreSQL + Redis）
└── README.md                     # 本文档
```

## 贡献指南

1. **选择或创建议题**：在提交拉取请求之前，请先在 Issues 区域查找是否存在相关议题。若无，请新建一个议题并描述你希望修复的问题或新增的功能，等待维护者确认方向。

2. **派生仓库并创建特性分支**：从主仓库的 `main` 分支派生个人副本，并在本地创建以 `feature/` 或 `fix/` 为前缀的分支名称，例如 `feature/add-timeout-config`。

3. **编写测试与代码**：所有新增逻辑必须包含对应的单元测试或集成测试。测试用例需覆盖正常路径与至少两个异常边界条件。代码需通过 ESLint 与 Prettier 检查，提交前可运行 `npm run lint` 与 `npm run test` 进行验证。

4. **更新文档与示例**：若你的变更涉及配置项、API 行为或界面交互，请同步更新 `docs/` 下对应的用户文档或架构说明，并在拉取请求描述中注明文档变更的位置。

5. **提交拉取请求**：推送到派生仓库后，向主仓库的 `main` 分支发起拉取请求。请求描述中需引用关联议题编号，列出变更点摘要，并勾选自检清单（测试通过、文档更新、无合并冲突）。维护者将在 7 个工作日内进行审核。

## 常见问题

**问：链接健康检查的频率是多少？是否会对目标站点造成压力？**

答：默认配置下，每个链接每日检查不超过 4 次，间隔不低于 6 小时。检查请求仅包含 GET 头部与超时设置，不下载完整页面内容（仅读取状态码与响应头）。对于高频率更新的数据域（如实时比分类），可单独配置更短间隔，但系统强制单链接最小间隔为 30 分钟。所有检查均遵守 robots.txt 中的 Crawl-delay 指令，若目标站点未声明则采用 2 秒请求间隔。

**问：如何导入现有的大批书签或浏览器收藏夹？**

答：系统支持通过 API 批量导入 CSV 文件（列映射：标题、URL、标签、分类），也提供脚本 `scripts/import-browser-bookmarks.js` 用于解析 Chrome 或 Firefox 导出的 HTML 书签文件。导入前建议使用 `--dry-run` 参数预览解析结果，避免误覆盖已有索引。

**问：如果收录的链接失效，系统会如何处理？**

答：当连续三次健康检查均返回 4xx 或 5xx 状态码时，该链接会被自动标记为 “unstable” 并移出默认视图。系统同时会向配置的管理员邮箱发送告警邮件，并在下一轮检查中降低频率至每 24 小时一次，直到人工确认恢复或手动删除。失效链接不会从数据库中物理删除，仅隐藏，以便后续追溯。

## 许可证

MIT License

Copyright (c) 2026 ChronoLink Navigator Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
