# HankLian Project

HankLian Project 是一个面向赛事数据分析与前瞻预测的开源技术资源聚合平台，专注于提供高可用、结构化的赛事数据接口、历史比分回溯、实时赛果同步以及多维度统计分析工具。该项目主要服务于数据科学家、量化分析师、体育赛事研究者以及金融风控领域中需要外部事件因子建模的开发团队。

项目核心定位为“赛事数据中间件”，通过标准化输出与可插拔的解析引擎，降低开发者从非结构化赛事页面中提取有效信息的复杂度。HankLian Project 不提供终端用户界面，所有功能均通过 RESTful API 与 Python SDK 对外暴露，适用于高频数据抓取、批量历史数据回测以及实时预警系统集成等企业级应用场景。

## 功能概览

- **实时赛果同步引擎** 支持多数据源并行抓取，毫秒级延迟内完成比分更新与状态变更推送，并提供数据一致性校验中间件。

- **历史比分批量回溯** 提供按赛季、按联盟、按时间区间的批量历史数据导出功能，支持 CSV 与 Parquet 格式输出，适配大数据离线分析链路。

- **积分榜动态聚合计算** 基于原始比赛数据自动生成实时积分榜，支持自定义积分规则（胜平负权重、净胜球系数等），可嵌入业务系统作为独立微服务。

- **前瞻预测特征工程模块** 内置常用赛事特征衍生算子（主客场胜率、近期状态指数、交锋历史熵值），可直接输出结构化特征表供机器学习管道调用。

- **多源数据融合校验机制** 针对同一赛事的不同数据来源进行交叉验证，自动标记冲突字段并生成可信度评分，保证下游分析的数据质量。

- **定时任务编排控制台** 提供基于 CRON 表达式的抓取任务调度接口，支持动态启停、频率调整以及失败重试策略配置，降低运维干预成本。

- **标准化 API 网关输出** 所有数据接口遵循 OpenAPI 3.0 规范，内置请求频率限制与鉴权中间件，可水平扩展至多机房部署架构。

## 应用场景

- **量化投研策略回测** 量化团队可将历史比分与积分数据导入因子分析框架，结合价格波动数据构建事件驱动型交易模型，验证特定赛事结果对关联资产的影响系数。

- **媒体内容自动化生产** 体育资讯平台通过实时赛果推送接口自动生成战报草稿，结合积分榜变化动态更新排名图表，减少人工编辑的重复劳动并提升发稿时效性。

- **风险控制外部因子接入** 金融风控系统定时拉取赛事前瞻分析结果，将其作为非金融类情绪因子纳入贷后管理模型，用于评估特定行业的外部环境波动风险。

- **学术研究与教学实践** 高校实验室使用历史数据集进行图网络模型训练或时间序列预测课题研究，同时可作为数据库课程中 ETL 流程设计的实战案例素材。

- **数据中台基础数据源补全** 企业数据中台通过标准化接口将赛事数据纳入统一数据湖，与用户行为、供应链等内部数据关联，探索跨界数据价值挖掘路径。

## 快速开始

以下步骤指导您在本地环境中完成 HankLian Project 的源码克隆、依赖安装与服务启动。

```bash
# 1. 克隆项目仓库
git clone https://github.com/hanklian-project/hanklian-core.git
cd hanklian-core

# 2. 安装 Python 依赖（推荐使用虚拟环境）
python -m venv venv
source venv/bin/activate  # Linux/macOS
# venv\Scripts\activate   # Windows
pip install -r requirements.txt

# 3. 初始化配置文件
cp config.example.yaml config.yaml
# 编辑 config.yaml 填写必要的数据源端点与认证信息

# 4. 运行数据库迁移（若启用持久化存储）
python manage.py migrate

# 5. 启动开发服务
python manage.py runserver --host 0.0.0.0 --port 8080
```

服务启动后，可通过 `http://localhost:8080/api/v1/health` 验证运行状态。详细 API 调用示例请参考文档导航中的开发者手册。

## 安装要求

生产环境部署前，请确保基础设施满足以下最低依赖要求。推荐使用 Linux 内核 5.x 以上操作系统，并保障网络出口可访问外部数据源端点。

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 / 3.10 / 3.11 | 核心运行环境，建议使用 pyenv 管理多版本 |
| PostgreSQL | 13.0 及以上 | 用于持久化存储历史数据与任务状态，支持 TimescaleDB 扩展 |
| Redis | 6.2 及以上 | 用作缓存层与分布式任务队列 Broker |
| RabbitMQ | 3.9 及以上 | 可选，用于高可靠性异步任务分发（Celery 后端） |
| Docker | 20.10 及以上 | 容器化部署方案推荐，提供一致的运行时环境 |
| Nginx | 1.20 及以上 | 生产环境反向代理与静态资源服务（可选） |
| 网络带宽 | 出方向 ≥ 10 Mbps | 保证多数据源并发抓取的吞吐量，建议配置独立公网出口 |
| 系统时区 | UTC / Asia/Shanghai | 所有时间戳统一为 UTC 存储，显示层可按需转换 |

## 文档导航

项目文档按角色与使用阶段分层组织，下表列出了主要文档目录及其解决的问题，帮助您快速定位所需内容。

| 层面 | 目录/文件 | 回答的问题 |
|------|-----------|------------|
| 用户入门 | `docs/quickstart.md` | 如何最快体验数据获取流程？API 密钥如何申请？首次调用返回什么结构？ |
| 开发者参考 | `docs/api_reference_v3.md` | 每个接口的路径参数、请求体范例、错误码定义以及分页规则是怎样的？ |
| 运维手册 | `docs/deployment/production_guide.md` | 如何配置高可用集群？日志轮转策略如何设置？监控指标如何接入 Prometheus？ |
| 数据字典 | `docs/data_dictionary.md` | 比分字段、球队编码、联赛 ID 的完整映射关系是什么？数据更新频率如何？ |
| 贡献规范 | `CONTRIBUTING.md` | 提交 PR 的流程是什么？代码风格检查如何执行？新增数据源适配器的模板在哪里？ |
| 常见问题 | `docs/faq.md` | 遇到数据缺失、延迟或格式异常时如何自行排查？如何联系维护团队？ |

## 资源列表

本项目的运行与持续迭代依赖以下外部信息资源，各链接按功能类别分组陈列。所有链接均保留原始格式，请根据实际网络环境配置访问策略。

**赛事数据分析主站**

- <code>hanklianqianzhan.asia</code>

**技术指标与积分统计**

- <code>hanklianjishibifen.asia</code>

**深度分析报告**

- <code>hanklianfenxi.asia</code>

**实时赛果发布**

- <code>hanklianbisaijieguo.asia</code>

**专项联赛数据**

- <code>eluosichaojiliansai.asia</code>

**积分榜聚合服务**

- <code>echaojifenbang.asia</code>

**前瞻预测数据服务**

- <code>bisaiyucefenxi.asia</code>

## 项目结构

项目采用领域驱动设计（DDD）分层架构，核心模块与外围基础设施严格隔离。以下为源码目录树及其职责说明。

```
hanklian-core/
├── api/                           # 对外接口层（路由、中间件、序列化器）
│   ├── v1/                        # API 版本 v1 实现
│   │   ├── endpoints/             # 具体路由端点函数
│   │   ├── schemas/               # Pydantic 请求/响应模型
│   │   └── validators/            # 自定义校验逻辑
│   └── middleware/                # 鉴权、限流、日志中间件
├── core/                          # 核心业务领域层（与框架无关）
│   ├── entities/                  # 领域实体（赛事、球队、比分、积分榜）
│   ├── value_objects/             # 值对象（联赛编码、时间区间、统计指标）
│   ├── repositories/              # 仓储接口定义（抽象数据访问）
│   └── services/                  # 领域服务（比分计算、特征衍生、校验引擎）
├── infrastructure/                # 基础设施实现层
│   ├── data_sources/              # 外部数据源适配器（HTTP 抓取、解析、重试）
│   ├── persistence/               # 数据库实现（PostgreSQL ORM 模型、迁移脚本）
│   ├── cache/                     # 缓存实现（Redis 客户端封装）
│   └── message_queue/             # 消息队列适配器（RabbitMQ 生产者/消费者）
├── orchestration/                 # 流程编排层（任务调度、工作流控制）
│   ├── schedulers/                # 定时任务调度器（APScheduler 配置）
│   ├── pipelines/                 # 数据处理管道（ETL 流程定义）
│   └── workers/                   # 异步任务执行器（Celery Worker 模块）
├── shared/                        # 公共工具与基础类
│   ├── config/                    # 配置加载与解析（YAML / 环境变量）
│   ├── logging/                   # 统一日志格式与输出级别控制
│   ├── metrics/                   # Prometheus 指标埋点与暴露
│   └── exceptions/                # 自定义异常层级与错误码映射
├── tests/                         # 测试套件（单元测试、集成测试、模拟数据）
│   ├── unit/                      # 领域层与服务层单测
│   ├── integration/               # 数据库与外部 API 联调测试
│   └── fixtures/                  # 测试用静态数据样本
├── scripts/                       # 运维与部署辅助脚本
│   ├── init_db.sql                # 数据库初始化脚本
│   └── health_check.sh            # 服务健康状态检测脚本
├── docs/                          # 项目文档（Markdown 源文件）
├── requirements.txt               # 生产环境依赖列表
├── requirements-dev.txt           # 开发环境额外依赖（测试、代码检查）
├── Dockerfile                     # 容器镜像构建定义
├── docker-compose.yml             # 本地开发环境编排（PostgreSQL + Redis + RabbitMQ）
├── Makefile                       # 常用命令快捷方式（install、test、lint、run）
└── pyproject.toml                 # 项目元数据与构建配置（PEP 621）
```

## 贡献指南

开源社区的贡献是项目持续演进的核心动力。我们欢迎任何形式的改进建议、缺陷报告或代码提交。请遵循以下标准化流程以确保协作效率。

1.  **问题追踪与讨论** 在提交代码前，请先在 GitHub Issues 中搜索是否已有相关讨论。若为新功能或缺陷，请创建 Issue 并详细描述复现步骤、预期行为与实际差异，维护团队会在 48 小时内给予初步反馈。

2.  **分支管理与开发环境** 请基于 `develop` 分支创建个人功能分支，分支命名遵循 `feature/功能简述` 或 `fix/缺陷编号` 格式。本地开发时务必激活虚拟环境并安装所有开发依赖（`pip install -r requirements-dev.txt`）。

3.  **代码风格与质量检查** 所有 Python 代码必须通过 `black` 格式化、`isort` 导入排序、`flake8` 静态检查以及 `mypy` 类型校验。提交前请在项目根目录执行 `make lint` 确认无警告或错误。

4.  **单元测试覆盖率要求** 新增或修改的代码必须附带对应的单元测试用例，且整体测试覆盖率不得低于 85%。运行 `make test` 可执行全部测试套件，并生成 HTML 格式覆盖率报告。

5.  **提交 PR 与代码审查** 功能开发完成后，向 `develop` 分支发起 Pull Request，并在描述中关联对应的 Issue 编号。PR 需要至少两名核心维护者批准后方可合并，合并前会触发 CI 流水线（测试、构建、安全扫描）。

## 常见问题

**Q1: 部署后首次启动任务调度器，发现部分数据源返回 HTTP 403 或 429 状态码，应如何处理？**

A: 该问题通常由目标数据源的反爬策略触发。首先检查 `config.yaml` 中 `request_headers` 字段是否包含合法的 User-Agent 与 Referer。其次，确认任务调度器的单次抓取并发数（`concurrency` 参数）是否过高，建议调整为 3 至 5 之间并启用随机延迟（`random_delay_seconds`）。若问题持续，请尝试更换出口 IP 或联系数据源管理员申请白名单。

**Q2: 历史比分回溯任务在处理超过 5 年数据时发生内存溢出，如何优化？**

A: 建议启用分页查询机制，调整 `batch_size` 参数为 1000 条/批次，并配合 `use_streaming` 开关开启流式处理模式。同时，确保 PostgreSQL 的 `work_mem` 和 `maintenance_work_mem` 参数已根据服务器内存调整（推荐设置为总内存的 10%）。对于超过 10 万条记录的回溯任务，建议使用 `--export-format parquet` 选项直接写入对象存储，避免中间结果驻留内存。

**Q3: 自定义积分榜规则后，重新计算的历史排名与预期不符，如何排查？**

A: 项目提供了 `debug.calculate` 命令行工具，可针对单场比赛或单个赛季输出详细的积分计算过程与中间变量。请执行 `python manage.py debug.calculate --season 2025 --team-id <ID> --verbose` 查看每个步骤的权重应用顺序。常见错误包括胜平负系数映射错误（检查 `scoring_rules.yaml` 中的 `win_points` / `draw_points` / `loss_points` 字段）以及未处理扣分规则（如净胜球为负数时的修正逻辑）。

## 许可证

HankLian Project 采用 MIT 许可证开源发布。您可以在遵守许可证条款的前提下自由使用、修改、分发本软件，包括用于商业目的。完整许可证文本请参见项目根目录下的 `LICENSE` 文件。

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
