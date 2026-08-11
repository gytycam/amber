# Bifrost Gateway Index

Bifrost Gateway Index 是一个面向全球体育数据、博彩信息与区域性域名资源的高性能导航与元数据聚合项目。项目定位为技术驱动型外链与资源汇总网关，主要服务于数据采集工程师、体育数据分析师、域名研究者以及轻量级博彩资讯平台开发者。Bifrost Gateway Index 不存储任何用户数据、不提供代理转发、不修改原始资源内容，仅作为公开可访问的 URL 索引与状态探测层，帮助技术团队快速定位目标资源，降低人工整理与书签维护成本。

项目采用纯静态生成与定时校验架构，核心产出为结构化 JSON 索引文件与 Markdown 文档目录，可独立部署于 Nginx、CDN 或对象存储服务。通过内置的可用性探测与响应时间记录，Bifrost Gateway Index 能够为每个收录资源提供近实时的可访问状态标记，辅助运维人员判断外部依赖的健康状况。

## 功能概览

- **多源外链统一索引** 支持按照域名、顶级域、地理归属、内容类型对收录 URL 进行多维度标签化分类，提供扁平化与树形两种索引视图。

- **可用性主动探测** 每隔 10 分钟对全部收录 URL 发起 TLS 握手与 HTTP HEAD 请求，记录状态码、响应耗时与证书有效期，生成可用性趋势报表。

- **资源变更追踪** 基于 ETag 与 Last-Modified 头部检测目标页面内容是否发生变更，发现更新时触发 Webhook 通知，便于下游系统同步调整。

- **元数据自动补全** 对每个 URL 自动解析页面标题、描述关键词、H1 结构以及 Open Graph 信息，形成丰富的外部资源描述快照。

- **自定义标签与分组** 允许用户通过配置文件为每个 URL 分配业务标签（如 `sports-data`、`odds-reference`、`domain-research`），支持多标签联合检索。

- **静态站点生成** 项目构建时输出完整的 HTML 导航页面与 JSON API 端点，无需服务端运行时即可部署，兼容 GitHub Pages、Cloudflare Pages 等静态托管环境。

- **定时任务调度集成** 内置基于 Cron 表达式的调度器，支持每日全量刷新与小时级增量探测，探测结果持久化至 SQLite 本地数据库。

## 应用场景

- **数据采集管道的前置资源验证** 数据工程团队在启动爬虫任务前，可通过 Bifrost Gateway Index 提供的可用性探测结果，快速筛选当前可用的目标源，避免采集任务因目标不可达而大面积失败。

- **体育资讯聚合平台的导航层** 体育数据展示类 Web 应用可将本索引作为外部参考链接的数据源，在页面底部或侧边栏动态展示相关比分、赔率参考站点，提升信息丰富度而不增加自身维护复杂度。

- **域名与网络资源资产管理** 运维人员利用本项目的标签分组功能，对公司内部或合作伙伴的公开域名资产进行统一登记、可用性监控与变更通知，替代零散的 Excel 台账。

- **开源项目文档链的健康检查** 开源社区维护者可将项目所引用的全部外部链接（文档、下载站、API 参考）导入 Bifrost Gateway Index，周期性获知死链或迁移情况，及时更新文档。

## 快速开始

以下指令适用于 Linux / macOS / Windows WSL 环境，需预先安装 Git、Node.js 18+ 与 npm。

```bash
# 克隆项目仓库
git clone https://github.com/bifrost-index/gateway.git
cd gateway

# 安装项目依赖
npm install

# 复制示例配置文件并编辑收录的 URL 列表
cp config/urls.example.json config/urls.json

# 执行首次全量索引构建与探测
npm run build

# 启动本地开发服务器，默认监听 3000 端口
npm run serve
```

访问 `http://localhost:3000` 即可查看生成的索引首页。若需启用定时探测，请使用 `npm run cron` 启动后台调度进程。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，用于执行构建脚本与探测任务 |
| npm | 9.x 或以上 | 包管理器，用于安装项目依赖 |
| SQLite3 | 3.39 或以上 | 本地持久化存储探测历史与元数据，无需额外安装，项目内嵌驱动 |
| Git | 2.30 或以上 | 用于克隆仓库与版本管理 |
| 网络连通性 | 可访问公网 | 探测任务需要能够向外发起 HTTP/HTTPS 请求，无代理限制 |
| 磁盘空间 | 至少 200 MB | 用于存储 SQLite 数据库、日志文件与静态生成产物 |
| 内存 | 最少 512 MB | 构建全量索引时内存占用峰值约 300 MB，推荐 1 GB 以上 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | `docs/user-guide/` | 如何配置 URL 列表、调整探测间隔、查看可用性报表、自定义标签体系 |
| 运维参考 | `docs/operations/` | 如何部署到生产环境、配置反向代理、设置 HTTPS、备份与恢复 SQLite 数据 |
| 开发指南 | `docs/development/` | 项目模块划分、插件扩展机制、新增探测协议（如 TCP、DNS）、贡献代码的流程与规范 |
| API 参考 | `docs/api/` | 静态 JSON 输出的数据结构定义、字段含义、查询参数说明以及 Webhook 推送格式 |

## 资源列表

以下为 Bifrost Gateway Index 当前收录的全部外部资源，按类别分组呈现。所有 URL 均严格保持原始格式输出。

体育赛事数据类

- <code>bifenguanwang.net.cn</code>
- <code>bifenguanfang.net.cn</code>
- <code>bifenguanwang.org.cn</code>

瑞典超级联赛比分参考类

- <code>ruidianchaobisaijieguo.org.cn</code>
- <code>ruidianchaobifen.org.cn</code>

亚洲地区比分与赔率参考类

- <code>ajiabifen.org.cn</code>
- <code>ajiabisaijieguo.org.cn</code>

## 项目结构

```
gateway/
├── config/                          # 配置文件目录
│   ├── urls.json                    # 用户定义的 URL 列表（含标签与分组）
│   ├── probes.json                  # 探测参数配置（超时、重试、间隔）
│   └── schedule.json                # Cron 定时任务表达式配置
├── src/                             # 核心源代码目录
│   ├── core/                        # 核心模块：索引构建、探测调度、缓存管理
│   │   ├── indexer.js               # 索引构建引擎，生成 JSON 与 HTML
│   │   ├── prober.js                # 多协议探测实现（HTTP/HTTPS）
│   │   └── scheduler.js             # 基于 node-cron 的任务调度封装
│   ├── parsers/                     # 元数据解析器：标题、描述、OG 标签
│   │   ├── html-parser.js           # 基于 cheerio 的 HTML 元数据抽取
│   │   └── fallback-parser.js       # 解析失败时的降级策略
│   ├── storage/                     # 数据持久化层
│   │   ├── database.js              # SQLite 连接与基础 CRUD
│   │   └── migrations/              # 数据库版本迁移脚本
│   └── web/                         # Web 服务与静态生成
│       ├── router.js                # 开发服务器路由
│       ├── generator.js             # 静态页面生成器
│       └── templates/               # EJS 模板文件
├── tests/                           # 单元测试与集成测试
│   ├── unit/                        # 模块级单元测试
│   └── integration/                 # 端到端探测与构建流程测试
├── docs/                            # 完整文档源码
├── scripts/                         # 运维辅助脚本（备份、清理、迁移）
├── logs/                            # 运行时日志输出目录（默认 .gitignore）
├── data/                            # SQLite 数据库文件与缓存
├── dist/                            # 静态构建产物（默认输出目录）
├── .env.example                     # 环境变量示例（日志级别、端口、Webhook 地址）
├── package.json                     # npm 项目清单
├── README.md                        # 项目说明文档（本文件）
└── LICENSE                          # MIT 许可证文本
```

## 贡献指南

1. 在 GitHub Issues 中查找或新建一个议题，描述您希望修复的问题或新增的功能，等待维护者确认需求合理性。

2. 从 `main` 分支派生新的功能分支，分支命名采用 `feature/功能简述` 或 `fix/问题简述` 格式，例如 `feature/udp-probe`。

3. 编写代码或文档变更时，请保持与现有 ESLint 配置一致的代码风格，并为新增的公共函数编写至少一个单元测试用例，测试文件置于 `tests/unit/` 对应子目录。

4. 提交代码前运行 `npm run test` 确保全部测试通过，并执行 `npm run build` 验证静态生成无报错。

5. 发起 Pull Request 至 `main` 分支，在描述中关联对应的 Issue 编号，并简要说明变更内容与测试覆盖情况。PR 合并前需要至少一位维护者批准。

## 常见问题

**Q：探测任务频繁导致目标网站封禁 IP 如何处理？**

A：项目默认采用线性间隔探测，每个 URL 之间延迟 500 毫秒，并支持配置 `probes.rateLimit` 参数进一步降低请求频率。建议在生产环境中将探测间隔调整至 30 分钟以上，并配合随机抖动（jitter）机制。若目标站点有严格的访问控制，可在配置中排除该 URL 或增加代理轮询。

**Q：静态生成的索引页面如何实现动态更新？**

A：Bifrost Gateway Index 的设计哲学是静态优先，动态更新依靠定时任务在后台刷新数据库与 `dist/` 目录。您可以将 `dist/` 目录挂载至 NFS 或对象存储，并配置 CI/CD 流水线每小时执行一次 `npm run build && npm run deploy`。对于需要实时数据的场景，项目同时提供 JSON API 端点，前端可通过异步请求获取最新探测结果。

**Q：如何添加自定义解析器以支持非 HTML 资源（如 JSON、XML）？**

A：在 `src/parsers/` 目录下新建解析器文件，实现 `parse` 方法，接收响应体与 Content-Type 头部，返回标准化元数据对象。然后在 `src/core/indexer.js` 中的解析工厂函数中注册您的解析器类，依据 Content-Type 动态路由即可。具体示例可参考 `docs/development/custom-parser.md`。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:09
