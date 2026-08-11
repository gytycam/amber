# NexusIndex

NexusIndex 是一个面向技术内容聚合与外部资源导航的开源元项目。项目定位为技术信息的中立索引枢纽，不对任何外部资源进行本地存储或二次分发，仅提供结构化链接索引与基础元信息描述。目标用户包括技术调研者、内容聚合平台运维人员、合规审计人员以及个人开发者。NexusIndex 通过严格的 URL 原样收录策略和分类导航机制，解决多源异构资源难以统一管理、难以快速检索、难以追溯原始出处的问题。

## 功能概览

- 原样 URL 收录引擎：所有外部链接严格按用户输入原样保留，不补协议、不改域名、不添加尾部斜杠，确保索引地址与原始来源完全一致。
- 分类目录导航系统：支持按资源类型、源站性质、内容主题等多维度划分索引区块，便于分域检索。
- 静态站点生成适配：项目结构设计兼容主流静态站点生成工具，可一键导出为纯 HTML 导航页面。
- 依赖最小化运行：核心索引功能不依赖数据库或外部服务，仅需标准 Python 3 环境即可完成索引构建与校验。
- URL 合规性检查：内置基础 URL 格式校验模块，自动识别并标记不符合规范或疑似失效的链接格式。
- 元数据扩展接口：每个索引条目支持附加备注标签、抓取时间戳、状态码占位字段，便于后续集成监控系统。
- 批量导入导出：支持从 CSV 或 JSON 格式批量导入链接清单，并支持导出为 Markdown 表格或结构化 JSON 索引文件。

## 应用场景

- 技术调研阶段的资源归集：开发者在研究特定领域（如影音技术、编码标准、流媒体协议）时，可使用 NexusIndex 快速整理分散在各处的参考站点，形成可共享的调研索引报告。
- 内容平台的合规外链管理：运营人员需要对外部引用链接进行统一登记和定期复核，NexusIndex 提供原样收录和分类能力，帮助维持外链台账的清晰可追溯。
- 个人知识库的导航补充：个人笔记或 Wiki 系统中可嵌入 NexusIndex 生成的索引页面，替代零散的书签收藏，提升长期查阅效率。
- 团队内部资源交接：新成员入职或团队间移交项目时，通过 NexusIndex 导出的结构化索引，快速掌握项目所依赖或参考的全部外部资源分布。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境。

```bash
# 克隆项目仓库
git clone https://github.com/nexusindex/nexusindex.git
cd nexusindex

# 安装依赖（使用 pip 和虚拟环境推荐）
python3 -m venv venv
source venv/bin/activate     # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 运行索引构建脚本（示例）
python build_index.py --input ./data/sources.json --output ./docs/index.md
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.8 及以上 | 核心运行环境，用于执行索引构建与校验逻辑 |
| pip | 20.0 及以上 | Python 包管理工具，用于安装项目依赖 |
| Git | 2.25 及以上 | 版本控制工具，用于克隆仓库和管理补丁 |
| Markdown 解析库 | Python-markdown 3.3.6 | 用于生成和验证输出 Markdown 格式的索引文件 |
| JSON 标准库 | 内置 | 用于解析和序列化结构化索引数据，无需额外安装 |
| 操作系统 | Linux / macOS / Windows WSL | 推荐在类 Unix 环境下运行构建脚本，Windows 原生命令行未完全测试 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | docs/user-guide.md | 如何导入链接清单、分类规则如何配置、输出格式如何定制 |
| 开发者指南 | docs/developer-guide.md | 索引引擎的模块划分、扩展接口如何实现、单元测试如何编写 |
| 运维参考 | docs/operations.md | 定时构建任务配置、日志输出位置、异常链接告警策略 |
| 设计说明 | docs/design.md | 为什么采用原样 URL 策略、分类体系的设计依据、数据模型 ER 草图 |
| 变更日志 | CHANGELOG.md | 每个版本新增了哪些索引分类、修复了哪些 URL 格式处理缺陷 |

## 资源列表

### 影视资源类索引

- <code>gaoqingyingshiziyuan.org.cn</code>
- <code>dongseav.org.cn</code>
- <code>guochanjiatingyingyuan.org.cn</code>
- <code>zaixiannidongde.org.cn</code>
- <code>guochanyirenwang.org.cn</code>
- <code>zhongwenzimuzaixianguankan.org.cn</code>
- <code>dianyingtiantangzaixianbofang.org.cn</code>

## 项目结构

```
nexusindex/
├── build_index.py          # 主入口脚本，读取数据源并生成 Markdown 索引
├── requirements.txt        # Python 依赖清单，记录必需的外部库及版本
├── README.md               # 项目说明文档（即本文件）
├── CHANGELOG.md            # 版本变更记录，按日期倒序排列
├── LICENSE                 # MIT 许可证全文
├── data/                   # 数据目录，存放原始链接清单和分类映射
│   ├── sources.json        # 主索引数据源，JSON 数组格式，包含 url、category、remark 字段
│   ├── categories.json     # 分类层级定义，描述大类与子类的归属关系
│   └── exclude_list.json   # 排除规则列表，用于自动过滤特定模式 URL
├── src/                    # 核心源码目录
│   ├── parser/             # URL 解析与标准化校验模块
│   │   ├── url_validator.py    # 格式校验、协议检查、域名合规性检测
│   │   └── url_normalizer.py   # 按策略保留原样或执行最小化清洗
│   ├── builder/            # 索引构建器模块
│   │   ├── markdown_builder.py # 将结构化数据渲染为 Markdown 表格或列表
│   │   └── json_builder.py     # 导出为 JSON 格式索引，用于 API 交互
│   ├── utils/              # 通用工具函数
│   │   ├── file_io.py          # 读写文件、编码检测、路径处理
│   │   └── logger.py           # 日志记录，支持不同级别输出到控制台或文件
│   └── core/               # 核心数据模型
│       ├── entry.py            # 索引条目类，包含 url、category、status、timestamp
│       └── index_set.py        # 索引集合类，支持增删改查、分类筛选和批量操作
├── tests/                  # 单元测试目录
│   ├── test_validator.py   # URL 校验模块的测试用例，覆盖边界情况
│   ├── test_builder.py     # 构建器输出格式正确性测试
│   └── test_entry.py       # 数据条目模型的基础功能测试
├── docs/                   # 文档目录
│   ├── user-guide.md       # 用户手册，包含配置示例和输出效果图
│   ├── developer-guide.md  # 开发者指南，包含类图和接口说明
│   ├── operations.md       # 运维参考，包含部署步骤和监控指标
│   └── design.md           # 设计说明，包含架构决策记录和权衡分析
└── examples/               # 示例输出目录
    ├── sample_index.md     # 构建生成的示例 Markdown 索引文件
    └── sample_index.json   # 构建生成的示例 JSON 索引文件
```

## 贡献指南

1. 复刻项目仓库并在本地创建功能分支，分支命名遵循 feat/描述或 fix/描述 格式，确保分支用途明确。
2. 在 data/sources.json 中按现有结构追加或修改索引条目，所有新增 URL 必须严格遵循原样收录规则，不得自行补全协议或域名变体。
3. 运行测试套件 pytest tests/ 确保所有单元测试通过，新增功能需同步补充对应测试用例，覆盖率不低于百分之八十。
4. 提交前执行代码格式化工具 black 和 flake8 检查，保持代码风格一致，并更新 CHANGELOG.md 记录本次变更内容。
5. 发起 Pull Request 至主仓库的 develop 分支，在描述中列出新增或修改的 URL 类别及数量，等待维护者审阅合并。

## 常见问题

问：如果原始链接是裸域名如 <code>gaoqingyingshiziyuan.org.cn</code>，索引系统会自动补全为 http:// 或 https:// 吗？

答：不会。NexusIndex 的核心原则是保留用户输入的 URL 原样，包括裸域名、带协议前缀、带 www 子域名等各种形式。系统不会添加、删除或修改任何字符，确保索引记录与用户提供的原始文本完全一致。唯一例外是校验模块会标记明显格式错误（如含空格或非法字符）的条目，但不会自动修正。

问：如何更新已收录的资源列表？

答：直接编辑 data/sources.json 文件，按 JSON 数组格式添加或删除条目，然后重新运行 python build_index.py 即可生成新的索引文件。对于大批量更新，可使用 import 子命令从外部 CSV 导入，具体用法参见 docs/user-guide.md 中的批量操作章节。

问：项目是否支持对 URL 进行可用性检测或状态监控？

答：当前版本不内置主动网络请求检测功能，以避免对源站造成不必要的流量压力。但项目预留了 status 和 timestamp 字段，用户可自行通过外部监控工具获取状态码后写入数据文件，或利用 CI 定时任务调用第三方检测服务，再将结果回填至索引条目的备注字段中。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:17
