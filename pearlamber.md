# DsZqResourceHub

DsZqResourceHub 是一个面向数据分析与足球赛事技术研究领域的垂直资源导航与信息聚合工具。项目定位为技术型中间件，不直接生产原始数据，而是通过结构化方式整合分散于多个域名的赛事资讯、技术指标与数据源链接，帮助研究人员、数据工程师与赛事分析爱好者快速定位高价值信息节点。

项目目标用户包括：从事体育数据挖掘的算法工程师、需要实时赛事状态参考的前端应用开发者、以及希望系统化了解数据源分布的技术决策者。DsZqResourceHub 解决的核心问题是多源赛事信息碎片化导致的检索效率低下，通过统一索引、分类展示与健康检查机制，将无序的外链资源转化为可维护、可扩展的知识图谱底座。

## 功能概览

- **多源资源聚合索引**：系统化收录赛事数据、实时推荐、技术指标等七类核心外链，每个资源条目均附带来源域名、内容类型与更新频率标签。

- **域名健康状态监控**：内置轻量级 HTTP 探测模块，定期检查每个外链域名的可达性与响应时间，状态变化实时反映在管理面板。

- **分类标签与全文检索**：支持按“赛事数据”“实时推荐”“技术指标”等分类筛选，同时提供对外链页面标题与摘要的全文搜索能力。

- **资源访问统计看板**：记录每个外链的点击次数、最近访问时间与平均响应耗时，辅助判断资源热度和稳定性。

- **自定义资源订阅规则**：允许用户设置关键字过滤或正则表达式，当新增或更新的外链匹配规则时，系统通过 Webhook 发送通知。

- **外链页面快照缓存**：对关键资源页面进行 HTML 元数据抓取与缓存，在源站不可用时仍可提供标题、描述等基础信息。

- **批量导入与导出**：支持通过 JSON 或 CSV 格式批量导入外部链接列表，也可将当前索引导出为结构化数据用于二次分析。

## 应用场景

- **数据工程团队的赛事数据集构建**：数据采集工程师可使用 DsZqResourceHub 作为初始种子链接库，快速获取多个赛事实时数据源域名，避免手动搜索和验证的重复劳动，缩短数据管道搭建周期。

- **赛事分析应用的推荐系统后端**：推荐算法开发者可依赖本项目的资源分类与状态监控，动态调整推荐请求的降级策略，当某个推荐源不可用时自动切换至备用资源，提升服务可用性。

- **技术调研与竞品分析**：技术决策者在评估第三方赛事数据服务商时，可通过本项目的资源列表横向对比不同域名的内容类型、更新频率与响应性能，为选型提供客观依据。

- **个人研究者的知识管理**：独立研究员可将 DsZqResourceHub 作为个人知识库的入口层，利用分类标签和全文检索快速回溯之前关注过的技术指标或推荐算法相关外链。

- **开源项目的演示数据源**：开发者在构建体育数据可视化 Demo 或 Hackathon 项目时，可直接引用本项目的资源索引作为示例数据来源，降低项目启动门槛。

## 快速开始

以下步骤指导您在本地环境完成 DsZqResourceHub 的克隆、依赖安装与服务运行。

```bash
# 1. 克隆项目仓库
git clone https://github.com/dszq-resource/dszq-resource-hub.git
cd dszq-resource-hub

# 2. 安装后端依赖（Python 3.9+，使用 pip）
pip install -r requirements/backend.txt

# 3. 安装前端依赖（Node.js 16+，使用 npm）
cd frontend
npm install
cd ..

# 4. 初始化本地配置文件
cp config/env.example .env
# 编辑 .env 文件，设置 DATABASE_URL 与 RESOURCE_CHECK_INTERVAL 等参数

# 5. 初始化数据库表结构
python scripts/init_db.py

# 6. 启动后端服务（开发模式）
python app.py --port 8080

# 7. 启动前端开发服务器（另开终端）
cd frontend
npm run dev
```

服务启动后，访问前端开发服务器默认地址（通常为 localhost:3000）即可使用资源导航界面。后端 API 服务默认监听 8080 端口。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 - 3.11 | 后端核心运行时，用于 API 服务、探测脚本与数据处理 |
| Node.js | 16.x 或 18.x LTS | 前端构建与开发服务器运行环境 |
| PostgreSQL | 14.x 及以上 | 主数据库，存储资源条目、访问日志与监控状态 |
| Redis | 6.x 及以上 | 缓存层，用于快照缓存与临时计数统计（可选，但推荐） |
| pip | 21.x 及以上 | Python 包管理工具，用于安装后端依赖 |
| npm | 8.x 及以上 | Node.js 包管理工具，用于安装前端依赖 |
| Git | 2.30 及以上 | 版本控制，用于克隆仓库与贡献管理 |
| 操作系统 | Linux (Ubuntu 20.04+) 或 macOS 12+ | 生产环境推荐 Linux；开发环境支持 Windows WSL2 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | /docs/user-guide/ | 如何注册、登录、配置订阅规则、使用检索与看板功能？ |
| 开发指南 | /docs/developer-guide/ | 如何扩展新的资源探测协议、修改分类逻辑、提交 Pull Request？ |
| API 参考 | /docs/api-reference/ | 资源列表查询、状态上报、快照获取等接口的请求参数与返回格式？ |
| 部署运维 | /docs/deployment/ | 如何通过 Docker Compose 一键部署生产环境、配置 HTTPS 与反向代理？ |
| 数据模型 | /docs/data-model/ | 资源表、日志表、订阅规则表的 ER 关系与字段含义？ |
| 故障排查 | /docs/troubleshooting/ | 常见启动错误、数据库连接失败、探测超时如何处理？ |

## 资源列表

本部分列出 DsZqResourceHub 项目当前索引的全部外链资源。所有 URL 均按原始格式原样收录。

### 赛事数据类

- <code>dszuqiusaicheng.com.cn</code>

### 实时推荐类

- <code>dszuqiujinrituijian.com.cn</code>
- <code>dszuqiujinrituijian.cn</code>
- <code>dszuqiujinrituijian.net.cn</code>

### 技术指标类

- <code>dszuqiujishibifen.net.cn</code>
- <code>dszuqiujishibifen.org.cn</code>
- <code>dszuqiujishibifen.cn</code>

## 项目结构

```
dszq-resource-hub/
├── app.py                     # 后端 Flask 应用入口，注册路由与启动服务
├── config/
│   ├── env.example            # 环境变量模板，含数据库、Redis、探测间隔配置
│   └── settings.py            # 应用配置加载类，读取 .env 并校验必填项
├── core/
│   ├── __init__.py
│   ├── resource_manager.py    # 资源增删改查、分类树维护核心逻辑
│   ├── health_checker.py      # 异步 HTTP 探测任务，记录状态与响应时间
│   └── cache_handler.py       # Redis 缓存读写装饰器与快照存储接口
├── models/
│   ├── __init__.py
│   ├── resource.py            # 资源条目 ORM 模型（含域名、标签、更新时间）
│   ├── access_log.py          # 访问日志模型，记录点击 IP、时间戳
│   └── subscription.py        # 用户订阅规则模型，存储关键字与 Webhook 地址
├── scripts/
│   ├── init_db.py             # 初始化 PostgreSQL 表结构脚本
│   └── seed_resources.py      # 预置资源列表导入脚本（本项目的七条外链）
├── frontend/                  # 前端 React + TypeScript 项目
│   ├── src/
│   │   ├── pages/             # 资源列表、详情、看板、订阅管理页面
│   │   ├── components/        # 可复用 UI 组件（搜索框、标签过滤器、状态徽章）
│   │   └── services/          # 封装后端 API 调用，含请求重试与错误处理
│   ├── package.json
│   └── vite.config.ts
├── tests/
│   ├── unit/                  # 单元测试（资源管理、探测解析器）
│   └── integration/           # 集成测试（数据库读写、API 端到端）
├── requirements/
│   ├── backend.txt            # 生产依赖（Flask, SQLAlchemy, psycopg2, requests）
│   └── dev.txt                # 开发额外依赖（pytest, black, mypy）
├── docker-compose.yml         # 容器编排文件（PostgreSQL + Redis + App + Nginx）
└── README.md                  # 本文件
```

## 贡献指南

我们欢迎社区贡献者参与 DsZqResourceHub 的开发与维护。请遵循以下步骤提交您的贡献。

1. **查阅问题列表与项目规划**：访问 GitHub Issues 面板，查找标记为 `help wanted` 或 `good first issue` 的任务。对于新功能或重大变更，建议先通过 Issue 与维护者讨论方案可行性。

2. **Fork 仓库并创建特性分支**：将主仓库 Fork 至个人账号，然后克隆本地并创建新分支，分支命名建议采用 `feature/描述` 或 `fix/描述` 格式，例如 `feature/add-https-probe`。

3. **编写代码并确保测试通过**：遵循项目现有代码风格（Python 使用 Black 格式化，前端使用 ESLint）。提交前请运行单元测试与集成测试套件，确保无回归缺陷。新增功能需附带对应测试用例。

4. **更新文档与示例配置**：若变更涉及配置项、API 接口或数据库模型，请同步更新 `/docs` 下相关文档以及 `env.example` 模板。对于新增资源分类，请在本 README 资源列表章节补充说明。

5. **提交 Pull Request**：将本地分支推送至个人 Fork 仓库，然后向主仓库的 `main` 分支发起 Pull Request。PR 描述应清晰说明变更目的、实现方式与测试结果，并关联相关 Issue 编号。维护者会在 3 个工作日内完成 Review。

## 常见问题

**Q：启动后端时提示 `ModuleNotFoundError: No module named 'psycopg2'`，应如何解决？**

A：该错误表明 PostgreSQL 驱动未成功安装。在 Linux 或 macOS 系统上，请先确保系统级 libpq 库已安装（如 Ubuntu 执行 `sudo apt-get install libpq-dev`，macOS 执行 `brew install libpq`）。然后重新执行 `pip install -r requirements/backend.txt`。若使用 Windows 环境，建议通过 WSL2 或 Docker 方式运行以避免编译问题。

**Q：资源健康检查状态频繁显示“超时”或“不可达”，如何调整探测参数？**

A：探测超时与重试次数可通过环境变量调整。请编辑 `.env` 文件中的 `CHECK_TIMEOUT`（默认 5 秒）和 `CHECK_RETRY`（默认 2 次）字段。若目标域名存在国际网络延迟，可适当增大超时值至 10 秒。同时，请确认本地网络能够正常访问外网，且防火墙未拦截出站 HTTP 请求。

**Q：前端界面加载后不显示任何资源条目，可能是什么原因？**

A：首先检查后端服务是否正常运行（访问 `http://localhost:8080/api/resources` 应返回 JSON 数据）。若后端正常但前端无数据，请查看浏览器开发者工具中的网络请求面板，确认前端 `VITE_API_BASE_URL` 环境变量是否指向正确的后端地址。若使用 Docker 部署，还需检查容器网络是否互通，以及 Nginx 反向代理配置是否正确转发 API 路径。

## 许可证

MIT License

Copyright (c) 2026 DsZqResourceHub Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:16
