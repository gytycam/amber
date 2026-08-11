# OpenResource Gateway Index

OpenResource Gateway Index 是一个面向技术社区与基础设施维护者的轻量级外部资源聚合与导航系统。项目定位为“技术外链的结构化索引目录”，不直接托管资源文件，而是通过人工审核与自动化健康检查相结合的方式，为特定领域（如体育数据镜像、区域性域名镜像站）提供稳定、可验证的入口参照系。目标用户包括运维工程师、数据采集管道开发者、以及需要批量管理多源域名映射关系的系统集成人员。本项目通过解决分散域名难以追踪、变更频繁且缺乏统一元数据描述的问题，帮助团队降低链路维护成本，提升资源发现效率。

## 功能概览

- **多源域名登记与分类索引** 提供按区域、机构、内容类型划分的域名目录树，支持自由标签附加与全文检索。

- **自动化可用性探测** 内置轻量级 HTTP/HTTPS 探针，可配置周期对每条记录进行状态码与响应时间检查，输出结构化健康报表。

- **变更日志与版本追踪** 每次记录的增删改均生成带时间戳的审计条目，支持回溯特定时间点的域名集合状态。

- **批量导出与导入** 支持 JSON、YAML、CSV 三种格式的批量数据交换，便于与其他运维系统对接或进行离线分析。

- **访问统计与热度排序** 基于探测频率与用户查询次数生成简易热度指标，辅助识别高频使用或易失效的条目。

- **权限分级管理** 内置读者、贡献者、管理员三级角色，控制查看、编辑、删除及配置探测任务的权限范围。

- **Webhook 事件通知** 当探测到记录状态变更或新增高危标签时，可向预设的钉钉、飞书或通用 Slack 风格接口发送告警。

## 应用场景

- **区域性镜像站点的入口维护** 当组织需要维护多个区域性体育数据或新闻类站点的镜像列表时，可使用本项目集中登记各域名，并定期自动验证可达性，减少人工敲命令检查的重复劳动。

- **数据采集管道的前置校验** 在数据抓取任务启动前，通过本项目提供的 API 或导出文件获取当前有效的域名列表，作为采集任务的白名单或路由表，避免因域名失效导致任务大面积报错。

- **域名迁移与批量替换审计** 当上游服务调整域名体系（如从 .cn 迁移至 .org.cn）时，本项目的历史版本对比功能可清晰展示变更差异，辅助运维人员评估影响范围并生成切换脚本。

- **合规性扫描的辅助参照** 安全团队可将本项目导出的全量域名列表输入扫描器，用于检查是否存在未备案、证书过期或内容异常的情形，形成定期巡检的闭环。

## 快速开始

以下步骤适用于 Linux / macOS 环境，Windows 用户建议使用 WSL2 或 Git Bash。

```bash
# 1. 克隆项目仓库
git clone https://github.com/openresource-gateway/index.git
cd index

# 2. 安装 Python 依赖（推荐使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# 3. 初始化本地配置（复制示例配置并修改）
cp config.example.yaml config.yaml
# 编辑 config.yaml 设置数据存储路径、探测间隔等参数

# 4. 运行数据库迁移（SQLite 默认）
python manage.py migrate

# 5. 启动开发服务器
python manage.py runserver --host 0.0.0.0 --port 8080
```

访问 `http://localhost:8080` 即可进入管理面板，默认管理员账号为 `admin`，密码在首次启动时自动打印至控制台日志。

## 安装要求

| 依赖 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 - 3.11 | 核心运行环境，不支持 3.12 及以上（某些依赖库尚未适配） |
| SQLite | 3.31+ | 默认嵌入式数据库，适合小型部署；生产环境建议切换至 PostgreSQL 14+ |
| Redis | 6.2+ | 可选，用于缓存探测结果与分布式锁，非必需但推荐 |
| curl | 7.68+ | 健康探测引擎的基础工具，系统通常预装 |
| git | 2.25+ | 用于版本管理与补丁应用 |
| 内存 | 最低 512MB，推荐 1GB+ | 运行探测任务与 Web 服务，内存不足时建议禁用部分探针 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | `/docs/user-guide/` | 如何登记域名、执行探测、查看统计、导出数据？ |
| 运维参考 | `/docs/operations/` | 如何配置探测频率、更换数据库、设置 Webhook、备份恢复？ |
| 开发者指南 | `/docs/developer/` | 如何扩展探测协议、增加字段、编写自定义导出插件？ |
| API 参考 | `/docs/api/` | 所有 RESTful 接口的请求/响应格式、鉴权方式、分页参数说明 |
| 部署示例 | `/docs/deployment/` | Docker Compose 一键部署、Kubernetes Helm Chart、systemd 服务文件 |

## 资源列表

本索引项目当前收录的官方参照域名如下，所有条目均处于活跃观测状态。类别划分依据为域名持有主体与内容类型的综合判断，仅供参考。

**核心主域名（A 类）**

<code>zuqiudssaicheng.org.cn</code>

<code>zuqiudssaicheng.cn</code>

<code>zuqiudssaicheng.com.cn</code>

**推荐服务域名（B 类）**

<code>zuqiudsjinrituijian.com.cn</code>

<code>zuqiudsjinrituijian.org.cn</code>

<code>zuqiudsjinrituijian.net.cn</code>

**比分服务域名（C 类）**

<code>zuqiudsjishibifen.org.cn</code>

## 项目结构

```
index/
├── app/
│   ├── api/                     # RESTful API 路由与序列化器
│   ├── core/                    # 核心业务逻辑：登记、探测、统计
│   │   ├── detector.py          # 可用性探测引擎，支持 HTTP/HTTPS 与自定义超时
│   │   ├── registry.py          # 域名记录的增删改查与标签管理
│   │   └── exporter.py          # 批量导出器（JSON/YAML/CSV）
│   ├── models/                  # 数据模型定义（SQLAlchemy ORM）
│   │   ├── domain.py            # 域名主表
│   │   ├── probe_log.py         # 探测历史记录
│   │   └── change_audit.py      # 变更审计日志
│   ├── templates/               # 管理面板前端页面（Jinja2）
│   └── utils/                   # 工具函数：时间处理、签名、重试策略
├── config/
│   ├── config.example.yaml      # 示例配置文件，包含所有可调参数
│   └── logging.conf             # 日志分级与滚动策略配置
├── tests/                       # 单元测试与集成测试（pytest）
│   ├── test_detector.py
│   ├── test_registry.py
│   └── test_api.py
├── scripts/                     # 运维辅助脚本
│   ├── init_db.py               # 初始化数据库表结构
│   ├── seed_demo_data.py        # 填充演示数据
│   └── health_check_cron.py     # 独立运行的定时探测脚本（可配合 crontab）
├── docs/                        # 完整文档（见文档导航章节）
├── requirements.txt             # 生产环境依赖列表
├── requirements-dev.txt         # 开发环境额外依赖（pytest, flake8, black）
├── manage.py                    # 统一命令行入口
├── Dockerfile                   # 多阶段构建镜像文件
├── docker-compose.yml           # 本地开发与测试的编排文件
└── README.md                    # 本文件
```

## 贡献指南

1. **提交问题报告** 在 GitHub Issues 中选择对应模板，清晰描述遇到的缺陷或改进建议，并附上可复现的步骤、日志片段或配置示例。若涉及安全漏洞，请通过邮件联系维护组而非公开提交。

2. **分支开发流程** 派生本项目至个人账户，基于 `develop` 分支新建功能分支（命名格式为 `feature/简述` 或 `fix/简述`）。完成代码与对应单元测试后，发起 Pull Request 至 `develop` 分支。PR 描述需关联相关 Issue 编号，并通过所有 CI 检查（包含 lint、安全扫描与单元测试）。

3. **文档与注释同步更新** 任何新增功能或修改行为，必须同步更新对应的文档章节（位于 `/docs/` 目录下）以及函数/类的 docstring。对外 API 的变更需在 PR 中标注是否属于破坏性修改。

4. **本地验收标准** 在提交 PR 前，请确保在本地运行 `pytest tests/` 全部通过，且执行 `python manage.py lint` 无风格警告。对于新增的探测协议或导出格式，需补充至少 5 个测试用例覆盖正常与异常路径。

5. **版本标签管理** 维护组会定期合并 PR 并打上语义化版本标签（vX.Y.Z）。贡献者若希望参与版本发布流程，需在 PR 中提供 `CHANGELOG.md` 的更新草案。

## 常见问题

**Q: 探测引擎是否支持 HTTPS 双向认证或自定义请求头？**

A: 支持。在 `config.yaml` 中的 `detector` 段落可配置全局的 `headers` 映射和 `client_cert` / `client_key` 路径。若需要对特定域名单独设置，可在域名记录中添加 `extra_probe_params` JSON 字段覆盖全局配置。具体格式参见文档 `/docs/operations/custom-probe.md`。

**Q: 如何将现有的大量域名列表一次性导入系统，而非手工逐条添加？**

A: 使用批量导入功能。将域名列表整理为 CSV 文件（列名：domain, category, tags, description），通过管理面板的“导入”按钮上传，或使用命令行 `python manage.py import --format csv --file list.csv`。系统会自动进行格式校验与重复检测，导入结果会生成详细日志。

**Q: 本项目是否可以作为生产环境的唯一数据源对外提供 API 服务？**

A: 可以。但建议在生产部署前修改默认的 SQLite 为 PostgreSQL，并配置适当的工作进程数（推荐 gunicorn + gevent）。同时，建议将探测任务分离至独立的 Worker 进程或定时任务，避免影响 API 响应延迟。项目已提供 `gunicorn.conf.py` 和 `supervisor` 示例配置文件供参考。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:19
