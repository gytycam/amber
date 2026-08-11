# HupuScore Archive

HupuScore Archive 是一个面向体育数据爱好者的开源技术资源聚合与导航系统。本项目并非数据采集或爬虫工具，而是一个精心编排的、可自托管的体育赛事数据源导航门户，旨在帮助开发者、数据分析师以及体育迷快速定位到稳定、公开的赛事比分与赛程信息来源。该项目的核心价值在于将分散的、非结构化的外部数据资源按照统一的元数据模型进行组织，并通过轻量级的 Web 界面或 API 方式提供查询与跳转服务，从而降低用户在多个数据源之间切换与验证的成本。

本项目定位为技术中间层，不存储任何赛事数据，仅对外部链接进行分类、标签化与可用性标记。目标用户包括但不限于：正在构建体育数据看板的开发者、需要进行赛事数据交叉验证的研究者、以及希望搭建个人比分通知系统的开源爱好者。通过 HupuScore Archive，用户可以在一份配置文件中管理所有数据源入口，并通过内置的可用性检测机制快速识别当前可用的服务端点。

## 功能概览

- **数据源分类导航**：按联赛类型、地区、数据粒度对收录的资源链接进行一级分类，支持用户自定义标签与分组。
- **可用性主动检测**：周期性对每个收录的 URL 执行 HTTP HEAD/GET 探活，标记异常源并在界面中高亮提示。
- **简洁只读 Web 面板**：提供基于 Flask 或静态 HTML 的仪表盘，以卡片或表格形式展示所有资源，支持关键字模糊搜索。
- **JSON API 输出**：暴露 `/api/sources` 端点，返回完整的资源列表及其实时状态，便于与其他系统集成。
- **配置热加载**：支持通过 YAML 或 JSON 配置文件动态增删数据源，无需重启服务即可生效。
- **访问日志记录**：记录用户查询与跳转行为，为后续分析热门数据源提供原始访问日志。
- **容器化一键部署**：提供 Dockerfile 与 docker-compose 示例，支持快速在任意 Linux 环境中拉起服务。

## 应用场景

- **个人数据看板集成**：开发者可将本项目部署为内部数据源网关，前端仪表盘通过 API 拉取资源列表，并为每个数据源生成可点击的跳转按钮，方便在比赛日快速查看不同平台的比分更新。
- **赛事数据交叉验证**：数据分析师在进行历史数据复盘或实时数据校准时，可以通过本导航页同时打开多个数据源，对比同一场赛事在不同平台上的比分与统计信息，以识别数据异常或延迟。
- **开源项目依赖资源引用**：其他开源项目（如赛事提醒机器人、数据可视化工具）可以在文档或配置中引用本项目的资源列表作为默认数据源，从而避免在每个项目中重复维护 URL 集合。
- **教育用途的教学示例**：计算机相关课程（如 Web 开发、网络编程）可借助本项目作为实战案例，演示如何设计一个基于外部资源索引的轻应用，并教授学生如何进行 HTTP 请求状态检测。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，假定已安装 Python 3.9 及以上版本与 Git。

```bash
# 1. 克隆仓库
git clone https://github.com/hupuscore-archive/hupuscore-nav.git
cd hupuscore-nav

# 2. 创建并激活 Python 虚拟环境
python3 -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate

# 3. 安装项目依赖
pip install -r requirements.txt

# 4. 初始化配置文件（拷贝示例配置）
cp config/sources.example.yaml config/sources.yaml

# 5. 启动开发服务器
python app.py
```

启动成功后，访问 `http://127.0.0.1:5000` 即可看到导航面板。API 端点可通过 `http://127.0.0.1:5000/api/sources` 访问。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 ~ 3.12 | 核心运行环境，3.13 暂未进行充分测试 |
| Flask | 2.3.x | Web 框架，用于提供面板与 API 服务 |
| PyYAML | 6.0+ | 用于解析 YAML 格式的配置文件 |
| requests | 2.31+ | 用于发送 HTTP 探活请求，检测数据源可用性 |
| pytest | 7.4+ | 单元测试框架（仅开发环境需要） |
| gunicorn | 21.2+ | 生产环境推荐使用的 WSGI 服务器（可选） |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|-----------|------------|
| 用户手册 | `docs/usage.md` | 如何添加或删除数据源？如何修改页面标题与描述？ |
| 运维指南 | `docs/deployment.md` | 如何使用 Docker 部署？如何配置 Nginx 反向代理？ |
| API 参考 | `docs/api.md` | API 的请求格式与返回字段含义是什么？如何限制访问频率？ |
| 开发指南 | `docs/development.md` | 如何运行测试？代码风格规范是什么？如何提交 PR？ |

## 资源列表

为便于管理，本项目将已收录的数据源按照域名主体进行分组展示。所有链接均源自公开互联网，项目本身不对链接内容的实时准确性负责，仅提供导航与状态检测能力。

### 虎扑篮球相关数据源

<code>hupuzuqiujishibifen.org.cn</code>

<code>hupuzuqiubisaijieguo.org.cn</code>

<code>hupuzuqiubifenwang.org.cn</code>

<code>hupuzuqiubifensaicheng.org.cn</code>

<code>hupuzuqiubifen.org.cn</code>

### 哈萨克斯坦篮球数据源

<code>hasakechaosaicheng.org.cn</code>

<code>hasakechaojishibifen.org.cn</code>

## 项目结构

```
hupuscore-nav/
├── app.py                      # 应用入口，初始化 Flask 并注册路由
├── requirements.txt            # Python 依赖列表
├── config/                     # 配置目录
│   ├── sources.yaml            # 用户自定义数据源配置文件（由 sources.example.yaml 复制生成）
│   └── sources.example.yaml    # 示例配置文件，包含全部 7 个默认数据源
├── core/                       # 核心逻辑模块
│   ├── __init__.py
│   ├── loader.py               # 加载 YAML 配置，解析为 Source 对象列表
│   ├── checker.py              # 异步/同步可用性检测器，维护状态缓存
│   └── logger.py               # 访问日志与系统日志配置
├── web/                        # Web 层模块
│   ├── __init__.py
│   ├── routes.py               # 定义 / 与 /api/sources 等路由处理函数
│   ├── templates/              # Jinja2 模板目录
│   │   ├── base.html           # 基础布局模板
│   │   └── index.html          # 首页面板，渲染资源卡片列表
│   └── static/                 # 静态资源
│       ├── css/                # 自定义样式（基于 Bootstrap 轻量定制）
│       └── js/                 # 前端交互脚本（搜索过滤、状态刷新）
├── tests/                      # 单元测试目录
│   ├── test_loader.py          # 测试配置加载与解析逻辑
│   └── test_checker.py         # 测试 HTTP 检测器超时与重试机制
├── docker/                     # 容器化相关文件
│   ├── Dockerfile              # 多阶段构建文件，基于 Alpine 镜像
│   └── docker-compose.yml      # 示例编排文件，包含应用与可选 Redis 缓存
└── docs/                       # 文档目录
    ├── usage.md                # 详细使用说明与配置字段解释
    ├── deployment.md           # 生产环境部署方案（gunicorn + systemd / Docker）
    ├── api.md                  # API 字段说明与示例响应
    └── development.md          # 开发环境搭建与贡献规范
```

## 贡献指南

1.  **问题跟踪与议题讨论**：在提交任何代码变更之前，请先在 Issues 区域查找是否存在相关议题。若无，请新建一个议题详细描述您希望解决的问题或希望新增的功能，并等待维护者确认方向。
2.  **派生仓库并创建特性分支**：将本仓库 Fork 至您的个人账号下，然后在本地从 `main` 分支切出一个新的功能分支，分支命名建议采用 `feat/` 或 `fix/` 前缀，例如 `feat/add-timezone-filter`。
3.  **遵循代码规范与测试**：代码应遵循 PEP 8 风格，所有新增函数或类必须包含 docstring。对于核心逻辑的变更，需在 `tests/` 下补充或更新对应的单元测试用例，并确保全部测试通过（执行 `pytest`）。
4.  **提交变更并签署开发者证书**：提交信息（commit message）应采用简洁清晰的英文描述，内容需说明变更的动机与影响。提交时需确保已签署项目的开发者原创证书（Developer Certificate of Origin），即提交中包含 `Signed-off-by` 行。
5.  **发起拉取请求**：将您的特性分支推送到您 Fork 的远程仓库，然后向本仓库的 `main` 分支发起 Pull Request。请在 PR 描述中关联对应的 Issue 编号，并简要说明测试覆盖情况。维护者将在 3 个工作日内进行审核。

## 常见问题

**问：为什么某些数据源在面板中显示为不可用（红色标记）？**

答：面板中的可用性状态由 `core/checker.py` 中的 HTTP 探活逻辑决定。系统会定期向每个数据源 URL 发送带有超时限制（默认 5 秒）的 GET 或 HEAD 请求。如果目标服务器返回 2xx 或 3xx 状态码，则标记为可用；若返回 4xx/5xx、超时、或 DNS 解析失败，则标记为不可用。由于外部服务可能具有反爬策略或临时维护，状态仅代表检测瞬间的结果，建议用户手动访问核实。您也可以调整 `config/sources.yaml` 中每个源下的 `timeout` 字段或 `check_interval` 全局配置。

**问：我可以直接在配置文件中添加非体育类的数据源吗？**

答：完全可以。本项目设计为通用的资源导航框架，并不限制数据源的类型。您可以在 `sources.yaml` 中自由增删任何 HTTP/HTTPS 链接，并为其赋予自定义的 `category`（分类）、`tags`（标签）和 `description`（描述）字段。Web 面板和 API 会自动适应新的数据结构，无需修改任何代码。但请注意，本项目不会对非体育数据源提供额外的解析或渲染支持。

**问：如何将服务部署到公网供团队内部使用？**

答：生产环境推荐使用 gunicorn 作为 WSGI 服务器，并结合 Nginx 进行反向代理以提供静态文件缓存与负载均衡。我们已在 `docs/deployment.md` 中提供了详细的 systemd 服务文件示例以及 Nginx 配置片段。如果您熟悉容器化，也可以直接使用 `docker/docker-compose.yml` 启动，该编排文件默认绑定宿主机 8000 端口，您只需在防火墙中放行该端口并配置域名解析即可。

## 许可证

MIT License

Copyright (c) 2026 HupuScore Archive Contributors

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

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
