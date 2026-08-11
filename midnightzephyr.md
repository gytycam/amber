# RizhiLink Aggregator

RizhiLink Aggregator 是一个专注于体育赛事数据聚合与实时信息分发的开源中间件项目。项目定位为技术型数据管道工具，面向需要从多源异构体育信息平台获取、清洗、合并及再分发数据的开发者与数据工程师。本项目不提供数据源本身，而是通过标准化的适配器接口，将分散的数据端点（RESTful API、静态页面、WebSocket 推送等）抽象为统一的数据视图，解决体育赛事信息获取过程中协议不统一、字段映射复杂、更新频率不一致以及源站可用性波动等工程问题。

目标用户包括个人开发者、开源数据项目贡献者、体育数据分析爱好者以及小型数据服务团队的架构师。项目通过可插拔的解析引擎与插件热加载机制，允许用户在不重启主进程的情况下动态注册新的数据端点或更新字段映射规则，从而显著降低因上游接口变动导致的维护成本。当前批次为项目第 11/567 批资源集成周期，本期集成重点覆盖东亚区域职业足球联赛的赛事结果与实时分析数据源。

## 功能概览

- **多协议适配器池**：内置 HTTP/HTTPS 客户端池与 WebSocket 长连接管理器，支持自动重试、超时控制和 TLS 指纹伪装，可同时管理 50 个以上的并发数据拉取任务。

- **声明式字段映射引擎**：通过 YAML 配置文件定义源 JSON/HTML 结构到内部标准化数据模型的映射关系，支持 JSONPath 与 XPath 表达式，并提供字段类型转换（字符串、整数、浮点数、日期时间、枚举）与默认值填充。

- **增量更新与变更检测**：基于内容哈希与时间戳双轨机制，仅当数据实体（如比赛结果、积分榜）发生实际变更时才触发下游通知，减少无效广播与存储写入压力。

- **数据质量校验管道**：内置校验规则链，支持范围校验、枚举白名单校验、逻辑一致性校验（如主队进球数与总进球数之和）以及空值/异常值哨兵检测，校验失败的数据将进入死信队列并记录结构化日志。

- **多格式输出适配器**：支持将标准化数据输出为 JSON、Avro、Parquet 格式，并可配置输出目标为本地文件系统、Kafka 主题或 Redis 发布/订阅频道。

- **健康检查与可观测性**：提供 Prometheus 指标暴露端点（/metrics），包括各数据源拉取延迟、成功率、解析错误计数与队列积压长度；同时支持配置存活探针（/health）与就绪探针（/ready）。

- **热加载配置中心**：监听本地配置目录或远程 Git 仓库变更，当映射配置文件更新后自动重新加载解析规则，无需停止服务。

## 应用场景

- **赛事结果数据仓库构建**：数据工程师可使用本项目作为 ETL 管道的摄取层，定时从多个区域联赛官方网站拉取比赛结果、积分榜与射手榜数据，经清洗转换后存入云数据仓库（如 ClickHouse 或 TimescaleDB），用于后续历史趋势分析与可视化大屏展示。

- **实时比分推送服务原型开发**：开发者可基于本项目的 WebSocket 客户端适配器连接多个比分推送端点，聚合后通过内置的 Redis 发布/订阅输出适配器转发至移动应用后端，快速搭建实时比分通知系统的原型版本。

- **数据源可用性监控与容灾切换**：运维人员可利用项目的健康检查与故障转移机制，配置主备数据源列表。当主数据源连续三次拉取超时或返回 HTTP 5xx 错误时，自动切换至备用数据源，并在日志中记录切换事件，保障下游数据流的连续性。

- **体育数据字段映射标准化实验**：数据分析师可在沙箱环境中使用本项目加载不同来源的 JSON 样本，通过声明式映射文件快速试验字段对齐策略，验证不同联赛数据结构之间的兼容性，为统一数据字典的制定提供工程验证基础。

- **开源数据管道教学案例**：计算机相关专业师生可将本项目作为数据工程课程的教学案例，展示异构数据源集成、适配器设计模式、配置驱动开发以及可观测性埋点等实际工程概念在真实业务场景中的应用。

## 快速开始

以下命令序列适用于 Linux 或 macOS 环境，Windows 用户请使用 WSL2 或 Git Bash。

```bash
# 步骤 1：克隆项目仓库
git clone https://github.com/rizhi-link/aggregator.git
cd aggregator

# 步骤 2：安装依赖（使用 pipenv 或 poetry）
pip install --upgrade pip
pip install pipenv
pipenv install --dev

# 或者使用 poetry
# pip install poetry
# poetry install

# 步骤 3：复制示例配置文件并启动服务
cp config/example.yaml config/local.yaml
pipenv run python -m rizhi_aggregator.main --config config/local.yaml --loglevel INFO
```

启动成功后，服务将默认监听本地的 8080 端口用于管理接口，数据拉取任务将根据配置文件的调度计划（默认每 60 秒执行一次）开始运行。您可以通过访问 `http://localhost:8080/health` 验证服务状态。

## 安装要求

| 依赖 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 或更高版本（不支持 3.9 及以下） | 核心运行时，项目大量使用 match-case 语法与类型提示新特性 |
| pipenv 或 poetry | pipenv>=2023.0.0 或 poetry>=1.5.0 | 依赖管理与虚拟环境隔离工具，二选一即可 |
| aiohttp | >=3.9.0 | 异步 HTTP 客户端/服务端框架，用于并发数据拉取与管理 API |
| pyyaml | >=6.0 | YAML 配置文件解析，用于声明式字段映射与任务调度定义 |
| jsonpath-ng | >=1.6.0 | JSONPath 表达式求值库，用于从 JSON 响应中提取嵌套字段 |
| lxml | >=4.9.0 | XPath 解析引擎，用于处理返回 HTML 结构的旧式数据端点 |
| prometheus-client | >=0.19.0 | Prometheus 指标暴露库，用于 /metrics 端点的监控数据聚合 |
| redis | >=5.0.0（Python 包版本） | 仅当使用 Redis 输出适配器时需要，可选依赖 |
| kafka-python | >=2.0.2 | 仅当使用 Kafka 输出适配器时需要，可选依赖 |
| pytest | >=8.0.0 | 仅开发与测试时需要，用于执行单元测试与集成测试套件 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting-started.md | 如何从零开始安装、配置并运行第一个数据拉取任务；如何验证数据管道是否正常工作 |
| 配置参考 | docs/configuration.md | 完整的 YAML 配置文件字段说明，包括数据源定义、映射规则、输出目标、调度策略与高级调优参数 |
| 适配器开发 | docs/adapter-development.md | 如何编写自定义适配器类以支持新的数据协议或认证方式；适配器生命周期钩子函数说明 |
| 故障排查 | docs/troubleshooting.md | 常见启动错误、网络超时处理策略、字段映射失败诊断方法以及日志级别调整建议 |

## 资源列表

本期集成批次共包含 7 个外部数据资源端点，均已完成初步连通性测试与字段映射样本编制。所有资源均属于东亚区域职业足球联赛数据范畴，具体如下：

联赛结果与官方信息类：

<code>rizhilianbisaijieguo.asia</code>

<code>ribenjliansaiguanwang.asia</code>

<code>ribenjliansai.asia</code>

<code>ribenzhiyezuqiujiajiliansai.asia</code>

实时分析与预测类：

<code>qiutanzuixinyuce.asia</code>

<code>qiutanzuixinfenxi.asia</code>

<code>qiutanjishibifen.asia</code>

以上资源在本项目中的适配器实现代码位于 `adapters/asia_football/` 目录下，每个资源对应一个独立的适配器类。使用者可根据实际需要，在配置文件中启用或禁用特定资源的拉取任务。请注意，本项目不保证上述外部资源的可用性、准确性或持续性，使用者应遵守各资源站点的服务条款。

## 项目结构

```
rizhi-aggregator/
├── adapters/                           # 所有数据源适配器实现
│   ├── asia_football/                  # 本期东亚足球联赛适配器集合
│   │   ├── __init__.py                 # 包初始化与适配器注册表
│   │   ├── result_adapter.py           # 比赛结果拉取适配器（对应 <code>rizhilianbisaijieguo.asia</code>）
│   │   ├── official_site_adapter.py    # 官方信息站适配器
│   │   └── analysis_adapter.py         # 分析与预测数据适配器
│   └── base/                           # 适配器基类与抽象接口定义
│       ├── http_adapter.py             # 通用 HTTP 拉取基类
│       └── websocket_adapter.py        # WebSocket 长连接基类
├── config/                             # 配置文件目录
│   ├── example.yaml                    # 完整配置示例，含注释说明
│   └── schema/                         # 配置 JSON Schema 校验文件
│       └── config_schema.json          # 用于 IDE 自动补全与验证
├── core/                               # 核心引擎模块
│   ├── engine.py                       # 任务调度引擎与事件循环管理器
│   ├── mapper/                         # 字段映射引擎
│   │   ├── jsonpath_mapper.py          # JSONPath 映射执行器
│   │   └── xpath_mapper.py             # XPath 映射执行器
│   └── pipeline/                       # 数据处理管道
│       ├── validator.py                # 数据质量校验器链
│       └── transformer.py              # 数据类型转换与标准化
├── output/                             # 输出适配器插件
│   ├── json_writer.py                  # JSON 文件输出
│   ├── redis_publisher.py              # Redis 发布/订阅输出
│   └── kafka_producer.py               # Kafka 消息队列输出
├── monitoring/                         # 可观测性组件
│   ├── metrics.py                      # Prometheus 指标定义与更新
│   └── logger.py                       # 结构化日志配置（JSON 格式）
├── tests/                              # 测试套件
│   ├── unit/                           # 单元测试，按模块划分
│   └── integration/                    # 集成测试，需外部网络环境
├── scripts/                            # 运维与开发辅助脚本
│   ├── hot_reload.sh                   # 配置热加载触发脚本
│   └── sample_data_generator.py        # 生成模拟数据用于本地调试
├── docs/                               # 项目文档（Markdown 格式）
├── Pipfile                             # pipenv 依赖声明文件
├── pyproject.toml                      # poetry 与项目元数据配置
└── README.md                           # 本文件
```

## 贡献指南

我们欢迎并鼓励社区贡献，无论是报告问题、提交代码还是完善文档。请遵循以下流程以确保协作顺畅：

1. 查阅现有 Issue 与 Pull Request：在提交新议题之前，请先访问项目的 GitHub Issues 页面搜索是否已有类似讨论。若发现重复议题，请在该议题下添加您的上下文信息而非新建。

2. 编写或更新测试用例：所有新功能或缺陷修复必须附带对应的测试用例。单元测试请放置于 `tests/unit/` 目录下，需要外部网络环境的集成测试请放置于 `tests/integration/` 目录下。测试需在 Python 3.10 与 3.11 环境中均通过。

3. 更新配置 Schema 与示例文件：如果您新增或修改了配置字段，请同步更新 `config/schema/config_schema.json` 中的 JSON Schema 定义，并在 `config/example.yaml` 中添加带注释的配置示例。

4. 提交 Pull Request：请从 `develop` 分支创建您的功能分支，提交后目标分支指向 `develop`。PR 标题应简洁描述变更内容，正文中请引用相关的 Issue 编号，并勾选 PR 模板中的自查清单（包括代码风格、测试覆盖率和文档更新）。

5. 签署开发者原产地证书：本项目采用 DCO（Developer Certificate of Origin）机制，每次提交均需包含 `Signed-off-by` 行，表示您有权贡献且同意本项目的许可证条款。您可通过 `git commit -s` 命令自动添加。

## 常见问题

**问：启动时提示 “Unsupported Python version”，但我的 Python 已经是 3.9 版本，应该如何解决？**

答：本项目明确要求 Python 3.10 或更高版本，因为核心代码依赖 `str.removeprefix()`、`int.bit_count()` 以及模式匹配语法，这些特性在 3.9 及以下版本中不存在或仅部分支持。请使用 `pyenv` 或 `conda` 安装 Python 3.10 以上版本，并在项目根目录执行 `pipenv --python 3.10` 重新创建虚拟环境。如果您必须使用 Python 3.9，请考虑 fork 项目并自行移植相关语法，但官方分支将不会提供兼容性补丁。

**问：某个数据源连续返回 HTTP 503 错误，导致任务队列阻塞，如何在不重启服务的情况下临时跳过该源？**

答：您可以使用管理接口的动态配置更新能力。向 `http://localhost:8080/config/source` 发送 PATCH 请求，将对应数据源的 `enabled` 字段设置为 `false`。配置变更将在下一次调度周期生效（最长延迟不超过一个完整拉取循环）。或者，您也可以直接编辑本地配置文件中的对应条目，然后执行 `scripts/hot_reload.sh` 脚本触发重载，服务会自动检测文件变更并重新加载配置，无需重启主进程。

**问：字段映射后，某些数字字段变成了字符串类型，导致下游消费出错，应该如何处理？**

答：请在映射配置中的 `fields` 部分为对应字段添加 `type` 属性，支持的转换类型包括 `integer`、`float`、`string`、`datetime` 和 `boolean`。例如，要强制将 `goals` 字段转为整数，可编写为 `goals: { path: "$.score", type: integer }`。如果转换失败（例如源数据包含非数字字符），该记录将进入死信队列，您可以通过 `/dead-letter` 管理端点查看失败原因。如需自定义异常处理逻辑，您可以扩展 `transformer.py` 中的 `ErrorHandler` 类并注册到管道配置中。

## 许可证

本项目采用 MIT 许可证进行开源。您可以自由使用、修改、分发本项目的源代码，包括用于商业目的，但需保留原始版权声明与许可声明。详细条款请参阅项目根目录下的 LICENSE 文件。

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
