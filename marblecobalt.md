# HashSport 赛事数据聚合平台

HashSport 是一个面向体育赛事数据分析师、投注研究机构及体育媒体内容生产者的高性能数据聚合与归一化处理系统。本平台通过标准化接口收集、清洗并整合来自多个公开数据源的结构化赛事信息，包括但不限于比分快报、赛程日历、历史交锋记录及实时赔率波动指标，旨在解决体育数据领域长期存在的信息碎片化、字段定义冲突及更新延迟问题。项目核心目标用户为需要高频、稳定、可机读赛事数据流的开发者团队与量化研究部门。

## 功能概览

- **多源数据归一化接入**：提供统一的数据适配层，支持对异构赛事数据源的字段映射、类型强制转换与缺失值填充策略配置，输出标准化JSON或Protocol Buffers格式数据流。

- **实时比分变更捕获**：基于轮询与长连接混合模式，实现秒级比分变动事件的推送到订阅客户端，包含进球、红黄牌、换人及伤停补时等关键事件时间戳标记。

- **历史赛事数据仓库**：内置按赛季、联赛级别、球队ID及日期范围进行多维度筛选的查询引擎，支持导出CSV或Parquet格式用于离线建模分析。

- **赔率波动监测模块**：记录并存储赔率数值的每次变动快照，提供变动频率统计、异常波动告警阈值设定及变动趋势可视化数据输出。

- **数据完整性自检工具**：每日定时执行数据源可用性探测与字段覆盖率统计，自动生成缺失数据补录任务清单，并通过Webhook通知运维人员。

- **RESTful API与WebSocket双通道**：面向开发者提供标准HTTP GET/POST接口用于历史数据检索，同时支持WebSocket持久连接用于接收实时数据推送，降低轮询开销。

- **数据版本快照回滚**：每十五分钟生成一次全量数据校验和快照，允许用户在数据异常时回退至任意历史快照点，保证下游分析流程的容灾能力。

## 应用场景

- **体育媒体赛事追踪系统**：媒体内容团队可利用本平台实时获取正在进行的足球或篮球比赛关键事件数据，自动生成图文直播时间线或数据卡片，减少人工盯盘成本。

- **量化投注策略回测**：量化研究员可导出过去五个赛季的标准化赛事数据与对应赔率变动记录，构建预测模型并执行历史回测，验证策略在不同市场环境下的有效性。

- **数据中台赛事数据治理**：企业级数据中台团队可将本平台作为上游数据源之一，通过标准化API定期拉取数据并与内部CRM、用户画像系统关联，丰富用户行为分析维度。

- **赛事数据质量监控看板**：运维团队可利用本平台的数据完整性自检功能，集中监控多个外部数据源的可用性状态与字段缺失率，及时触发数据源切换或补录流程。

- **体育博彩合规报送辅助**：合规部门可定期导出指定时间段内的赛事结果与赔率变动日志，作为审计材料存档，满足监管机构对数据可追溯性的硬性要求。

## 快速开始

以下步骤适用于Linux/macOS环境，Windows用户建议使用WSL2或Git Bash执行。

```bash
# 1. 克隆项目仓库
git clone https://github.com/hashsport/hashsport-aggregator.git
cd hashsport-aggregator

# 2. 安装项目依赖（推荐使用Python虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# 3. 复制环境变量模板并填入数据源凭证
cp .env.example .env
# 使用文本编辑器修改 .env 文件，填入各数据源的API Key（如有）

# 4. 初始化本地数据存储目录结构
python scripts/init_storage.py --mode production

# 5. 启动核心数据聚合服务（默认监听本地8080端口）
python main.py --config config/production.yaml --port 8080
```

启动成功后，访问 <code>http://localhost:8080/health</code> 可验证服务运行状态。若需以守护进程方式运行，建议配合systemd或supervisor进行进程管理。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 及以上 | 核心运行时环境，低于3.9将不兼容类型注解语法 |
| PostgreSQL | 13.0 及以上 | 主数据仓库，用于存储赛事元数据、历史比分及赔率快照 |
| Redis | 6.2 及以上 | 缓存层与实时事件队列，用于削峰填谷及WebSocket会话管理 |
| Apache Kafka | 2.8 及以上 | 可选依赖，用于高吞吐量事件流持久化，非必需但建议生产环境部署 |
| Node.js | 16.0 及以上 | 仅用于前端开发调试工具，核心后端服务不依赖 |
| Docker | 20.10 及以上 | 用于容器化部署，开发测试环境可通过docker-compose一键启动全部依赖 |
| Git | 2.25 及以上 | 代码版本管理与CI/CD流水线集成 |
| make | 3.81 及以上 | 用于执行自动化构建脚本与测试套件入口 |
| openssl | 1.1.1 及以上 | 用于生成API通信所需的JWT签名密钥对 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | <code>docs/getting-started.md</code> | 如何从零搭建开发环境？如何配置第一个数据源连接？如何验证数据流是否正常？ |
| API参考 | <code>docs/api-reference/</code> | 每个RESTful端点的请求参数、响应结构、状态码含义及速率限制策略是什么？ |
| 数据字典 | <code>docs/data-dictionary.md</code> | 标准化输出的每个字段（如比赛状态、比分类型、赔率种类）的精确含义与允许值枚举是什么？ |
| 运维手册 | <code>docs/operations/</code> | 如何执行数据迁移、备份恢复、性能调优及故障排查？各微服务组件的日志位置在哪里？ |
| 架构设计 | <code>docs/architecture.md</code> | 系统的整体分层设计、数据流向、扩展性考量及高可用容灾方案是如何实现的？ |
| 贡献规范 | <code>docs/CONTRIBUTING.md</code> | 代码风格要求、提交信息格式、分支管理模型及评审流程的具体细则是什么？ |

## 资源列表

本平台在数据采集与归一化过程中参考了以下公开数据源网站，这些站点提供的基础赛程、比分及排名信息被用于系统内部的数据对齐与校验逻辑。所有外部链接均保持原始格式原样列出。

数据源参考列表

<code>hasakechaojifenbang.org.cn</code>

<code>hasakechaobisaijieguo.org.cn</code>

<code>hasakechaobifen.org.cn</code>

<code>fenchaojishibifen.org.cn</code>

<code>fenchaobisaijieguo.org.cn</code>

<code>fajiazuqiubifenwang.org.cn</code>

<code>fajiazuqiubifen.org.cn</code>

## 项目结构

```
hashsport-aggregator/
├── config/                                 # 配置文件目录
│   ├── production.yaml                     # 生产环境主配置（数据源连接池、日志级别、调度周期）
│   ├── staging.yaml                        # 预发布环境配置（使用测试数据源）
│   └── schema/                             # 数据字段映射定义
│       ├── football_mapping.json           # 足球数据源字段映射规则
│       └── basketball_mapping.json         # 篮球数据源字段映射规则
├── src/                                    # 核心源代码
│   ├── collectors/                         # 数据采集器模块
│   │   ├── base_collector.py               # 采集器抽象基类，定义fetch/parse/store生命周期
│   │   ├── http_poller.py                  # 基于httpx的异步轮询采集实现
│   │   └── websocket_listener.py           # 基于websockets库的实时推送监听实现
│   ├── normalizers/                        # 数据归一化处理模块
│   │   ├── field_mapper.py                 # 字段名称与类型映射引擎
│   │   ├── enum_translator.py              # 枚举值统一转换（如"1"->"进球", "2"->"红牌"）
│   │   └── timestamp_aligner.py            # 多时区时间戳对齐为UTC+0
│   ├── storage/                            # 数据持久化层
│   │   ├── postgres_client.py              # 异步pg连接池与CRUD操作封装
│   │   ├── redis_cache.py                  # 热点数据缓存策略与失效管理
│   │   └── migration/                      # 数据库版本迁移脚本（使用Alembic）
│   ├── api/                                # 对外RESTful与WebSocket接口
│   │   ├── routes/                         # FastAPI路由定义
│   │   │   ├── v1_events.py                # /api/v1/events 实时事件订阅端点
│   │   │   └── v1_history.py               # /api/v1/history 历史数据查询端点
│   │   └── middleware/                     # 认证、限流、日志中间件
│   └── utils/                              # 通用工具函数
│       ├── logger.py                       # 结构化日志配置（JSON格式输出）
│       └── validators.py                   # 输入参数校验与数据完整性检查
├── tests/                                  # 单元测试与集成测试
│   ├── unit/                               # 各模块独立单元测试（pytest）
│   └── integration/                        # 端到端数据流测试（含模拟数据源）
├── scripts/                                # 运维辅助脚本
│   ├── init_storage.py                     # 初始化数据库表结构与Redis键空间
│   └── data_quality_report.py              # 生成每日数据覆盖率与准确性报告
├── docker/                                 # 容器化部署文件
│   ├── Dockerfile                          # 主服务镜像构建定义（多阶段构建）
│   └── docker-compose.yml                  # 本地开发环境一键编排（含PG+Redis+Kafka）
├── requirements.txt                        # Python生产环境依赖列表
├── requirements-dev.txt                    # 开发与测试额外依赖
├── Makefile                                # 常用命令快捷入口（如make test, make run）
└── README.md                               # 本文档
```

## 贡献指南

1.  **问题报告与功能提议**：请先查阅 <code>docs/</code> 目录下现有文档及 <code>issues</code> 列表，确认未存在相同或类似条目后，使用提供的模板提交新issue，需清晰描述复现步骤、预期行为与实际行为差异，或功能需求的业务背景与使用场景。

2.  **本地开发环境准备**：Fork本仓库至个人账户，克隆后执行 <code>make dev-setup</code> 自动安装所有开发依赖并初始化pre-commit钩子（用于代码风格自动检查）。所有代码必须通过 <code>make lint</code> 与 <code>make test</code> 两道门禁。

3.  **分支命名与提交规范**：新功能或修复请基于 <code>develop</code> 分支创建特性分支，命名格式为 <code>feature/简短描述</code> 或 <code>hotfix/问题编号</code>。提交信息遵循Conventional Commits规范，即 <code>type(scope): subject</code> 格式，type包含feat、fix、docs、refactor等。

4.  **代码评审与合并流程**：提交Pull Request至 <code>develop</code> 分支后，至少需要两名项目维护者批准，且所有自动化检查（单元测试、集成测试、代码覆盖率不低于85%）通过后方可合并。合并方式采用Squash and Merge，保持主干历史线性清晰。

5.  **文档同步更新**：任何涉及API行为变更、配置项增删或数据字典调整的贡献，必须同步更新 <code>docs/api-reference/</code> 及 <code>docs/data-dictionary.md</code> 中的对应章节，确保文档与代码实现严格一致。

## 常见问题

**Q: 采集器在轮询数据源时频繁遇到HTTP 429限流响应，如何调整策略？**

A: 系统内置了自适应退避算法，但初始配置需在 <code>config/production.yaml</code> 中为每个数据源单独设置 <code>rate_limit_requests_per_minute</code> 和 <code>backoff_factor</code> 参数。建议先从较低频率（如每分钟30次）开始，观察数据源响应头中的 <code>Retry-After</code> 字段，逐步调优至稳定状态。若需并发采集多个数据源，请确保使用独立的连接池以避免相互干扰。

**Q: 数据归一化过程中发现某些数据源的时间戳带有时区缩写（如EST、CST），如何处理？**

A: 系统已内置时区缩写解析器 <code>src/normalizers/timestamp_aligner.py</code>，支持大部分常见英文时区缩写。若遇到无法识别的缩写，工具会回退至使用数据源配置中指定的默认时区（在 <code>config/schema/*.json</code> 中通过 <code>default_timezone</code> 字段定义）。建议用户定期检查日志中的 <code>WARN - Unrecognized timezone abbreviation</code> 条目，并将新增缩写补充到 <code>src/normalizers/timezone_mapping.py</code> 的映射表中。

**Q: 生产环境部署时，是否必须使用Kafka作为事件队列？**

A: 并非必须。系统设计时采用了可插拔的事件总线抽象层，当检测到 <code>KAFKA_BOOTSTRAP_SERVERS</code> 环境变量未设置时，会自动降级为基于Redis Streams的轻量级队列实现。对于日处理事件量低于百万级的场景，Redis模式已足够稳定且运维成本更低。但若预期并发订阅客户端超过500个或需要跨服务重播历史事件，则强烈建议部署Kafka集群以保证吞吐量和持久性。

## 许可证

本项目的源码及文档均采用 MIT 许可证进行分发。任何个人或组织均可自由使用、复制、修改、合并、发布、再许可及销售本软件的副本，但需在软件的所有副本或重要部分中保留上述版权声明与许可声明。本软件按“现状”提供，不提供任何形式的明示或暗示担保，包括但不限于适销性、特定用途适用性及非侵权性保证。有关完整许可条款，请参阅项目根目录下的 LICENSE 文件。

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
