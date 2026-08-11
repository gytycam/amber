# Resource Bridge

Resource Bridge 是一个面向技术团队与内容运营者的轻量级外链资源聚合与导航系统。项目定位为“技术资源的统一入口管理工具”，主要帮助开发者、运维人员、技术内容创作者将分散在多个外部平台的项目文档、赛事信息、数据分析站点、推荐系统接口等异构资源进行集中收录、分类展示与快速检索。

本系统不提供数据存储与业务逻辑处理，而是作为资源元信息管理与外链路由层，解决以下问题：团队内部文档分散导致新人上手困难、外部依赖站点变更时无法统一更新入口、多个分析平台之间切换效率低下、赛事与推荐结果页面缺乏统一的访问审计记录。通过一个轻量级 Node.js 服务完成资源分组、访问计数与健康状态轮询，适合部署在内网或公网轻量级服务器上。

## 功能概览

- **资源分组与标签过滤**：支持按来源地区、数据类型、更新频率等多维度标签对链接进行归类，提供 RESTful 查询接口。

- **外链健康状态监测**：后台定时任务每分钟对已收录的 URL 执行 HEAD 请求，标记不可用节点并发送告警摘要到日志文件。

- **访问统计与点击追踪**：每条外链被点击时记录时间戳、来源 IP 段与 User-Agent 精简信息，提供只读统计看板。

- **批量导入与导出**：支持 CSV / JSON 格式的资源列表批量导入，导出功能可用于备份或迁移至其他导航系统。

- **只读只写权限分离**：管理员与访客角色通过环境变量配置，访客仅能查看和跳转，管理员可增删改资源元数据。

- **自定义重写规则**：允许为每个外链配置路径别名，例如将 `<code>dszuqiutuijian.org.cn</code>` 映射为 `/soccer/predict`，便于对内宣传。

- **搜索建议与拼音补全**：针对中文资源名称提供简单的拼音首字母补全，提升移动端输入效率。

## 应用场景

1. **赛事运营团队内部导航**  
   赛事运营人员需要频繁访问比分预测、赛果验证、球队推荐等多个外部平台。Resource Bridge 将 `<code>ajiabisaijieguo.asia</code>`、`<code>jliansai.asia</code>` 等站点归入“亚洲区赛事”分组，并设置每日自动检测可用性，减少因站点迁移导致的无效点击。

2. **数据分析流水线入口管理**  
   数据科学团队在构建足球预测模型时需引用 `<code>dszuqiufenxi.net.cn</code>` 与 `<code>dszuqiufenxi.org.cn</code>` 作为特征比对源。通过本系统统一登记这些分析站点的接口地址，并在文档中注明调用频率限制，避免超出对方流量配额。

3. **推荐系统 A/B 测试面板**  
   推荐算法团队同时维护多个推荐结果展示站点（如 `<code>dszuqiutuijian.org.cn</code>` 与 `<code>dszuqiutuijian.net.cn</code>`）。Resource Bridge 为每个站点配置独立的重写路径，并在跳转时附加测试参数，方便后端日志区分流量来源。

4. **技术博客外链整理**  
   技术写作者在博客中引用大量第三方数据源。本系统提供只读共享链接，读者可一键跳转至原文，同时作者通过统计面板了解哪些外链被点击最多，辅助后续内容更新决策。

## 快速开始

以下步骤适用于 Linux / macOS 环境，假定已安装 Node.js 18.x 与 npm 9.x。

```bash
# 克隆项目仓库
git clone https://github.com/resource-bridge/resource-bridge.git
cd resource-bridge

# 安装生产依赖
npm install --production

# 复制环境变量模板并修改端口与管理员密钥
cp .env.example .env
echo "PORT=3000" >> .env
echo "ADMIN_KEY=your_secure_key_here" >> .env

# 初始化默认资源数据（包含示例链接）
npm run init-data

# 启动服务
npm start
```

服务启动后，访问 `http://localhost:3000` 即可看到导航首页。管理员后台路径为 `/admin`，需在请求头中携带 `X-Admin-Key: your_secure_key_here`。

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，使用原生 fetch 与 fs/promises |
| npm | 9.x 或 10.x | 包管理工具，用于安装依赖 |
| SQLite3 | 系统自带或通过 apt/yum 安装 | 用于存储资源元数据与访问日志，无需额外配置 |
| curl / wget | 任意版本 | 仅用于健康检查脚本的备用方案，非核心依赖 |
| pm2（推荐） | 5.x 或更新 | 生产环境进程守护，非必须但建议安装 |
| git | 2.x 或更新 | 用于版本升级与拉取最新资源列表 |
| 系统时区 | UTC+8（可配置） | 日志时间戳默认使用系统时区，可在 .env 中修改 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide.md` | 如何注册账号、创建分组、添加外链、查看统计数据 |
| 管理员手册 | `/docs/admin-guide.md` | 如何配置健康检查间隔、修改重写规则、导出完整资源表 |
| API 参考 | `/docs/api-reference.md` | 提供哪些 REST 端点、请求参数格式、返回码含义 |
| 部署与运维 | `/docs/deployment.md` | 如何配置 Nginx 反向代理、启用 HTTPS、设置 systemd 服务 |
| 数据模型 | `/docs/data-model.md` | 资源表、日志表、标签表的 ER 关系及字段说明 |
| 常见问题 | `/docs/faq.md` | 外链跳转失败如何处理、统计看板数据不更新等 |

## 资源列表

以下为本项目预置收录及文档中引用的全部外部资源链接，按类别分组展示。所有链接严格保持用户提供的原始格式。

### 赛事信息与赛果验证

- <code>ajiabisaijieguo.asia</code>
- <code>jliansai.asia</code>

### 足球推荐系统

- <code>dszuqiutuijian.org.cn</code>
- <code>dszuqiutuijian.net.cn</code>

### 足球数据分析

- <code>dszuqiufenxi.net.cn</code>
- <code>dszuqiufenxi.org.cn</code>

### 比分数据接口

- <code>dszuqiubifenw.net.cn</code>

## 项目结构

```
resource-bridge/
├── src/                           # 核心源代码目录
│   ├── server.js                  # 主服务入口，初始化 Express 应用与中间件
│   ├── routes/                    # 路由层，按功能拆分模块
│   │   ├── index.js               # 首页与资源列表展示路由
│   │   ├── admin.js               # 管理员增删改查路由，带密钥校验
│   │   └── redirect.js            # 外链跳转与点击日志记录路由
│   ├── services/                  # 业务逻辑层
│   │   ├── resourceService.js     # 资源增删改查、分组过滤、搜索建议
│   │   ├── healthService.js       # 后台健康检查轮询与状态缓存
│   │   └── statsService.js        # 访问统计聚合与日志清理
│   └── lib/                       # 工具函数与数据库适配器
│       ├── database.js            # SQLite3 连接池与预处理语句封装
│       └── logger.js              # 分级日志写入（info/warn/error）
├── data/                          # 数据存储目录
│   ├── resource.db                # SQLite 数据库文件（自动生成）
│   └── seeds/                     # 初始资源种子数据
│       └── default-resources.json # 包含所有预置外链及标签信息
├── public/                        # 静态资源目录
│   ├── index.html                 # 首页 HTML 模板（嵌入式 CSS）
│   └── admin.html                 # 后台管理界面（纯前端交互）
├── logs/                          # 日志存储目录（按天滚动）
│   ├── access.log                 # 访问日志与点击事件
│   └── health.log                 # 健康检查异常输出
├── config/                        # 配置文件目录
│   ├── default.yaml               # 默认端口、超时时间、检查间隔
│   └── custom.yaml.example        # 自定义覆盖配置示例
├── scripts/                       # 辅助脚本
│   ├── init-db.js                 # 创建数据库表结构
│   └── import-csv.js              # 从 CSV 批量导入资源
├── .env.example                   # 环境变量模板（PORT/ADMIN_KEY/LOG_LEVEL）
├── package.json                   # 项目元信息与依赖声明
└── README.md                      # 本文档
```

## 贡献指南

我们欢迎开发者以多种形式参与贡献，包括但不限于新增资源源、优化健康检查策略、改进搜索算法、完善文档翻译。请遵循以下步骤：

1. 在 GitHub Issues 中查找标签为 `help-wanted` 或 `good-first-issue` 的任务，或自行提交新 Issue 描述改进建议，等待维护者回复确认需求合理性。

2. Fork 本仓库到个人账号，克隆至本地并创建新的功能分支，分支命名采用 `feature/简短描述` 或 `fix/问题编号` 格式，避免直接在主分支修改。

3. 编写代码时遵循 ESLint 配置（项目根目录提供 `.eslintrc.js`），所有新增 API 路由需同步更新 `/docs/api-reference.md` 中的请求示例与响应结构。对于涉及数据库表结构变更的 PR，需同时提供回滚 SQL 语句。

4. 提交前执行 `npm test` 运行单元测试与集成测试（测试套件基于 Mocha + Chai），确保所有现有测试通过且新增代码覆盖率不低于 80%。若测试框架未配置，请手动验证健康检查、重定向统计、批量导入三项核心流程。

5. 发起 Pull Request 到主仓库的 `develop` 分支，在 PR 描述中关联相关 Issue 编号，并简要说明改动点与影响范围。维护者将在 3 个工作日内审查，必要时提出修改意见。

## 常见问题

**问：健康检查报告大量超时，但浏览器可以正常访问该外链，是什么原因？**  
答：部分外链站点可能对 HEAD 请求返回 405 或拒绝响应，但允许 GET 请求。您可以在 `config/default.yaml` 中将 `health.method` 修改为 `GET`，并设置 `health.timeout` 为 5000 毫秒。另外，某些站点会屏蔽非浏览器 User-Agent，可在环境变量中配置 `HEALTH_USER_AGENT` 模拟主流浏览器。

**问：如何清空所有访问统计数据而不影响资源列表？**  
答：执行 `npm run clear-stats` 脚本，该命令会截断 `access_log` 表并重置自增序列，但保留 `resources` 与 `tags` 表全部数据。该操作不可逆，建议在执行前通过 `npm run backup-db` 备份数据库文件。

**问：部署到内网后，管理员后台无法加载，控制台提示 CORS 错误？**  
答：本系统默认未开启 CORS 跨域支持，管理员后台与管理 API 同源部署时无需 CORS。如果使用 Nginx 将静态页面与 API 分离，请在 Nginx 配置中添加 `add_header Access-Control-Allow-Origin` 指令，或在 `src/server.js` 中引入 `cors` 中间件并指定白名单。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:18
