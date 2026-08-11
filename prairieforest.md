# NovaLink 技术资源导航站

NovaLink 是一个面向开发人员、数据分析师和技术决策者的轻量级外链资源聚合与导航系统。该项目定位于对高频使用的技术赛事数据、实时比分服务、预测分析平台及专业对比工具进行集中式管理与快速路由，解决多源信息分散、检索路径冗长、数据可信度难以验证等实际问题。NovaLink 不存储任何原始赛事数据或用户隐私信息，仅作为结构化链接中间层，通过分类索引、状态监控和访问计数，帮助团队在数据获取环节节省约 40% 的查找时间。

目标用户包括数据竞赛参与者、体育数据分析爱好者、量化投研工程师以及需要快速接入第三方实时数据接口的后端开发人员。NovaLink 提供清晰的数据源分级、可用性探测和访问日志聚合，使技术团队能够以最低维护成本维护一套可靠的外链书签系统，并支持通过简单的 HTTP 健康检查实现对下游依赖服务的可用性追踪。

## 功能概览

- **分类链接索引** 按照赛事类型、数据服务商、分析工具等维度对原始 URL 进行标签化分组，支持多级目录和模糊搜索。
- **可用性主动探测** 每隔 10 分钟对注册链接执行 TCP/HTTP 层面可达性检查，超时阈值 5 秒，异常状态自动标记并写入告警日志。
- **访问统计与排序** 记录每个外链的点击次数、最后访问时间和平均响应耗时，支持按热度或响应速度排序输出。
- **只读转发中间件** 所有外部链接经过 NovaLink 的 `/goto/{id}` 端点进行 302 重定向，同时记录跳转日志，便于审计和反滥用。
- **元数据缓存层** 对频繁访问的链接信息（标题、描述、分类、SSL 证书有效期）进行本地内存缓存，TTL 为 6 小时，减少对上游源的依赖。
- **结构化健康报告** 每 30 分钟生成一份所有监控链接的可用性摘要报表，格式为 JSON，可被 Prometheus 或 DataDog 等监控系统采集。
- **RESTful 管理 API** 提供链接增删改查、批量导入导出、状态强制刷新等运维接口，支持 Basic Auth 鉴权。

## 应用场景

- **赛事数据聚合看板** 数据分析团队可将所有第三方赛事结果、赛程预测和排名链接统一纳入 NovaLink，在前端看板中以 iframe 或外链卡片形式集中展示，避免分析师在多个标签页间手动切换。
- **量化策略回测依赖管理** 量化工程师使用 NovaLink 管理历史比分、赔率变化和赔率对比服务的外部数据源地址，在回测脚本中通过 NovaLink API 动态获取当前可用的数据端点，实现数据源故障时的自动降级切换。
- **内部技术文档集成** 企业技术文档站点可将 NovaLink 作为“常用外部工具”章节的后端驱动，所有外链由 NovaLink 统一维护，文档页面只保留链接 ID，变更时无需重新构建静态站点。
- **爬虫任务调度辅助** 爬虫开发者在任务配置中引用 NovaLink 的链接分类标签，按类别批量获取待采集目标列表，同时利用可用性探测结果跳过当前不可用的采集源，提高任务成功率。
- **团队共享书签库** 技术团队可将 NovaLink 部署为内部共享书签中心，所有成员拥有统一的入口来访问各类技术赛事、预测和对比平台，新成员入职时无需手动收集散落的文档链接。

## 快速开始

以下步骤适用于 Linux/macOS 环境，要求已安装 Git、Go 1.21+ 及 SQLite3。

```bash
# 克隆代码仓库
git clone https://github.com/novalink-project/novalink.git
cd novalink

# 安装 Go 依赖模块
go mod download

# 复制示例配置文件并修改监听端口与探测参数
cp config.example.yaml config.yaml

# 构建二进制文件
make build

# 运行服务（默认监听 8080 端口）
./bin/novalink -config ./config.yaml
```

启动成功后，访问 `http://localhost:8080/api/v1/links` 即可获取当前已录入的全部链接列表。如需导入用户提供的初始链接集合，可使用 `make import` 命令并指定 CSV 文件路径。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Go 运行时 | 1.21 及以上 | 项目采用 Go Modules 管理依赖，泛型特性用于缓存和集合操作 |
| SQLite3 | 3.35 及以上 | 嵌入式数据库，存储链接元数据、访问日志和探测历史记录 |
| Git | 2.25 及以上 | 用于克隆仓库和管理子模块（如有） |
| GNU Make | 3.81 及以上 | 构建脚本和任务编排，用于编译、测试和打包 |
| curl / wget | 任意稳定版本 | 用于健康探测中的 HTTP 请求执行，需支持 SSL/TLS 1.2 |
| jq | 1.6 及以上 | 可选，用于命令行下解析 API 返回的 JSON 数据，便于调试 |

## 文档导航

| 层面 | 目录 / 文件 | 回答的问题 |
|---|---|---|
| 用户手册 | `docs/user-guide/` | 如何注册新链接、如何分类标签、如何查看可用性报表、如何配置通知 |
| 运维指南 | `docs/operations/` | 如何部署集群、如何备份 SQLite 数据、如何调整探测频率和告警阈值 |
| API 参考 | `docs/api-reference/` | 每个 REST 端点的请求方法、参数格式、响应结构及错误码含义 |
| 设计文档 | `docs/design/` | 整体架构图、缓存策略、探测调度算法、数据模型 ER 图及扩展性考量 |

## 资源列表

以下为用户提供的全部原始链接资源，按功能类别分组收录。所有链接均保持原始格式原样列出，未做任何协议补全或域名规范化处理。

赛事结果类

- <code>hanklianbisaijieguo.asia</code>
- <code>eluosichaojiliansai.asia</code>

积分与排名类

- <code>echaojifenbang.asia</code>

预测分析类

- <code>bisaiyucefenxi.asia</code>

对比工具类

- <code>bijiazhugongbang.asia</code>
- <code>bijiazhibo.asia</code>
- <code>bijiatuijian.asia</code>

## 项目结构

```
novalink/
├── cmd/                                # 主程序入口
│   └── novalink/                       # 服务启动引导包
│       └── main.go                     # 解析命令行参数、加载配置、启动 HTTP 服务
├── internal/                           # 内部私有包，不对外暴露
│   ├── cache/                          # 内存缓存实现（LRU + TTL）
│   │   ├── cache.go                    # 泛型缓存容器，支持任意类型的键值存储
│   │   └── metrics.go                  # 缓存命中率与驱逐计数统计
│   ├── probe/                          # 可用性探测调度器
│   │   ├── scheduler.go                # 基于 cron 表达式的定时探测任务管理
│   │   ├── http_checker.go             # HTTP/HTTPS 请求执行、状态码及耗时采集
│   │   └── result_store.go             # 探测结果写入 SQLite 的持久化逻辑
│   ├── api/                            # RESTful API 路由与处理器
│   │   ├── router.go                   # 注册所有端点，绑定中间件（日志、鉴权、限速）
│   │   ├── link_handler.go             # 链接的 CRUD、跳转、批量操作接口
│   │   └── report_handler.go           # 可用性报表、统计摘要输出接口
│   └── model/                          # 数据模型与数据库操作
│       ├── link.go                     # Link 结构体定义，包含 ID、原始 URL、分类标签等字段
│       ├── visit_log.go                # 访问日志模型，记录点击时间、来源 IP、User-Agent
│       └── migration.go                # 数据库表结构自动迁移（GORM 或原生 SQL）
├── pkg/                                # 可被外部导入的公共库
│   ├── config/                         # 配置文件解析（YAML + 环境变量覆盖）
│   └── logger/                         # 结构化日志封装（基于 slog，支持 JSON 和 text 格式）
├── configs/                            # 配置文件模板
│   ├── config.example.yaml             # 完整配置项示例，含注释说明
│   └── probes.example.yaml             # 预设探测任务示例（链接列表与频率）
├── scripts/                            # 运维辅助脚本
│   ├── import_csv.sh                   # 从 CSV 文件批量导入链接的 Shell 脚本
│   └── health_check.sh                 # 外部健康检查脚本，供监控系统调用
├── test/                               # 单元测试与集成测试
│   ├── integration/                    # 需要启动真实数据库和 HTTP 服务的集成测试
│   └── mock/                           # Mock 对象与测试辅助函数
├── docs/                               # 全部文档源文件（Markdown + PlantUML）
│   ├── user-guide/                     # 用户手册各章节
│   ├── operations/                     # 运维部署与故障处理文档
│   └── design/                         # 架构设计及技术决策记录（ADR）
├── Makefile                            # 构建任务定义：build, test, run, import, clean
├── go.mod                              # Go 模块依赖声明
├── go.sum                              # 依赖校验和文件
└── README.md                           # 项目总览（当前文档）
```

## 贡献指南

1. **选择或创建 Issue** 在 GitHub Issues 中查找未被认领的任务，或提交新的功能请求/缺陷报告，描述清晰且附带复现步骤（如适用）。获得维护者反馈后再开始编码，避免无效 PR。

2. **派生仓库并创建功能分支** 将主仓库派生至个人账户，然后克隆本地。基于 `develop` 分支新建以 `feature/` 或 `fix/` 为前缀的分支，例如 `feature/add-batch-delete-api`。禁止直接在 `main` 或 `develop` 上修改。

3. **编写代码与测试** 遵循项目代码风格（使用 `gofmt` 和 `golangci-lint`）。新增或修改功能必须附带对应的单元测试，测试覆盖率不得低于 80%。对于 API 变更，需同步更新 `docs/api-reference/` 中的 OpenAPI 描述文件。

4. **提交前本地验证** 运行 `make test` 确保所有测试通过，运行 `make build` 确认编译无报错。检查日志输出无敏感信息（如密码、Token）。提交信息使用规范格式：`<type>(<scope>): <subject>`，类型包括 feat, fix, docs, refactor, test, chore。

5. **发起 Pull Request** 推送分支至个人派生仓库，向主仓库的 `develop` 分支发起 PR。PR 描述中关联对应 Issue，并勾选自测清单。至少需要一名维护者审阅通过后，方可合并。CI 流水线会自动执行测试与静态分析，若失败需修复后重新推送。

## 常见问题

**Q1: 为什么某些链接在 NovaLink 中显示为不可用，但我直接在浏览器中可以访问？**

A: NovaLink 的探测机制默认使用 HEAD 请求并遵循 5 秒超时限制，且不携带浏览器中的 Cookie 或会话状态。部分网站可能对 HEAD 请求返回 405 或 403，或要求特定的 User-Agent 头。您可以在配置文件中调整探测方法（如改为 GET）、增加超时时间，或为特定链接配置自定义请求头。另外，某些站点有反爬策略，会屏蔽非浏览器的请求来源，此时可将探测源 IP 加入对方白名单或调整探测频率以避免触发限流。

**Q2: 如何备份 NovaLink 存储的所有链接数据与访问日志？**

A: NovaLink 默认使用 SQLite3 数据库，数据文件位于 `data/novalink.db`。您可以直接使用 `sqlite3 data/novalink.db .dump > backup.sql` 进行逻辑备份，或使用文件系统级别的复制命令（需先停止服务或使用 `PRAGMA wal_checkpoint` 确保 WAL 日志落盘）。对于生产环境，建议配置定时任务每日凌晨执行备份，并将备份文件上传至对象存储。若使用外部 PostgreSQL（通过编译标签切换），则使用对应数据库的原生备份工具。

**Q3: 能否在 NovaLink 中为同一个原始 URL 设置多个分类标签？**

A: 当前数据模型支持一对多的标签关联。您可以通过管理 API 的 `PUT /api/v1/links/{id}/tags` 端点提交一个标签数组，例如 `["赛事数据", "预测分析", "历史比分"]`。系统会在链接标签关联表中创建映射记录，并支持按任意标签组合进行筛选查询。在导入 CSV 时，可使用分号分隔的多标签列，例如 `赛事数据;预测分析`。

## 许可证

MIT License

Copyright (c) 2026 NovaLink Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
