# DSZQ 开源技术资源聚合平台

DSZQ 是一个面向全球开发者与数据爱好者的开源技术资源聚合与导航系统。本项目的核心定位并非代码库或框架，而是一个高质量外链与信息源的整理、验证、分类与检索平台。目标用户包括技术调研人员、数据采集工程师、运维架构师以及需要快速获取特定领域权威信息源的产品团队。通过机器可读的元数据描述与人工审核的条目注释，DSZQ 致力于解决互联网信息碎片化、来源不可靠、检索效率低下等长期痛点，为技术社区提供一个可自托管、可扩展、可协作的参考信息基底。

## 功能概览

- **多维度资源分类索引**：按领域、地域、数据粒度、更新频率等维度对收录的 URL 进行标签化组织，支持快速筛选与定向检索。

- **自动化可用性探测**：系统每日对收录的域名与路径执行 TLS 证书有效性、响应码、解析耗时等健康检查，并将异常状态可视化标记。

- **元数据增强注解**：每个资源条目可附加来源描述、数据格式说明、访问限制提示、备案信息等扩展字段，便于团队内部知识传递。

- **版本化变更追踪**：所有增删改操作均记录操作日志与时间戳，支持回滚至任意历史版本，满足审计与协作场景需求。

- **批量导入与导出**：支持通过 CSV、JSON Lines 格式批量导入外部链接清单，亦可按筛选条件导出为结构化数据文件，便于下游系统集成。

- **私有化部署与权限分级**：提供基于角色的访问控制（RBAC），支持公开只读、内部审核、管理员编辑三级权限体系，适配企业内部知识库场景。

- **自定义标签与收藏集**：用户可创建个人或团队级标签体系，将分散的资源聚合为专题收藏集，如“国内数据源”“赛事统计”“实时接口”等。

- **全文检索与模糊匹配**：基于倒排索引实现资源标题、描述、域名、标签的多字段检索，支持拼音首字母与同义词扩展。

## 应用场景

- **技术调研期的信息汇集**：当团队需要评估某垂直领域（如体育数据接口、区域资讯聚合）的公开数据源质量时，可通过本平台快速获取候选列表，并参考历史可用性记录与社区评论，大幅降低信息筛选成本。

- **数据采集任务的源管理**：数据工程师可将本项目作为外部依赖源清单的基准仓库，在采集脚本中通过 API 拉取最新活跃源列表，避免因硬编码 URL 导致的维护困难。

- **内部知识库的补充模块**：企业可基于本项目搭建私有的外部参考信息目录，将合同约定来源、合作伙伴数据地址、内部测试环境入口统一纳入管理，并与现有 Wiki 或 Confluence 进行双向链接。

- **开源社区的资源共建**：社区维护者可定期发起资源有效性审查活动，通过 Pull Request 形式更新失效条目，形成持续进化的社区信息资产。

- **自动化监控的配置底座**：运维团队可将本平台导出的 JSON 资源清单作为 Prometheus Blackbox Exporter 或自定义健康检查脚本的输入配置，实现对外部依赖的集中式可观测性。

## 快速开始

以下操作假定您已安装 Git 与 Node.js（v18 LTS 及以上版本）。本项目采用纯静态生成方案，无需额外数据库依赖。

```bash
# 1. 克隆代码仓库至本地
git clone https://github.com/dszq-community/resource-hub.git
cd resource-hub

# 2. 安装项目依赖（使用 npm 或 yarn）
npm install

# 3. 启动开发服务器，预览本地站点
npm run dev
```

执行上述命令后，终端会输出本地访问地址（通常为 <code>http://localhost:3000</code>）。若需构建生产环境静态文件，请使用 `npm run build`，产物默认输出至 `dist` 目录，可部署至任意静态托管服务。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|---|---|---|
| Node.js | v18.0.0 或更高 | 运行时环境，用于执行构建脚本与开发服务器 |
| npm | v9.0.0 或更高 | 包管理器，用于安装依赖项；也可使用 yarn 或 pnpm |
| Git | v2.30.0 或更高 | 版本控制工具，用于克隆仓库与提交变更 |
| 操作系统 | Linux / macOS / Windows（WSL 推荐） | 跨平台支持，但生产构建建议使用 Linux 环境 |
| 网络访问 | 可访问公共互联网 | 用于首次安装时下载 npm 包及后续资源验证探测 |
| 浏览器 | 支持 ES2022 的现代浏览器 | 用于访问管理界面与查看资源面板（Chrome / Firefox / Edge 最新版） |
| 磁盘空间 | 至少 200 MB 空闲空间 | 包含源代码、依赖及构建缓存 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | <code>/docs/user-guide/</code> | 如何注册账号、如何创建收藏集、如何提交新资源、如何查看健康状态 |
| 管理员手册 | <code>/docs/admin-guide/</code> | 如何审批资源提交、如何配置探测间隔、如何管理用户权限与角色 |
| 开发者指南 | <code>/docs/developer-guide/</code> | 如何二次开发插件、如何扩展元数据字段、如何编写自定义探测器 |
| API 参考 | <code>/docs/api-reference/</code> | 如何通过 REST API 获取资源列表、提交变更、查询历史记录 |
| 部署运维 | <code>/docs/deployment/</code> | 如何部署到生产环境（Nginx / Caddy / S3）、如何配置 HTTPS 反向代理 |

## 资源列表

### 主域名组

<code>dszuqiujifenbang.org.cn</code>

<code>dszuqiujifenbang.net.cn</code>

<code>dszuqiujifenbang.com.cn</code>

### 分析服务子域组

<code>dszuqiufenxigw.org.cn</code>

<code>dszuqiufenxi.cn</code>

### 赛事结果子域组

<code>dszuqiubisaijieguo.net.cn</code>

<code>dszuqiubisaijieguo.org.cn</code>

## 项目结构

```
dszq-resource-hub/
├── .github/                         # GitHub 社区配置文件（Issue / PR 模板）
│   ├── ISSUE_TEMPLATE/
│   └── workflows/                   # CI 自动构建与健康检查流水线
├── config/
│   ├── categories.json              # 资源分类体系定义（领域/子领域）
│   ├── probes.json                  # 自定义探测参数（超时、重试、期望状态码）
│   └── tags.json                    # 预置标签库与同义词映射
├── data/
│   ├── resources/                   # 核心资源条目存储（按年份/月份分片）
│   │   ├── 2026-01.json
│   │   └── 2026-02.json
│   ├── audit.log                    # 变更操作审计日志（追加写入）
│   └── snapshots/                   # 每日全量快照（用于版本回滚）
├── src/
│   ├── api/                         # RESTful API 实现（Express / Fastify）
│   │   ├── routes/
│   │   └── middleware/
│   ├── core/                        # 核心业务逻辑（资源校验、标签引擎、探测调度）
│   │   ├── validator.js
│   │   ├── tagger.js
│   │   └── scheduler.js
│   ├── frontend/                    # 前端管理界面（React / Vue）
│   │   ├── components/
│   │   ├── pages/
│   │   └── hooks/
│   └── workers/                     # 后台任务（健康检查、统计聚合）
│       ├── probe-worker.js
│       └── stats-worker.js
├── tests/                           # 单元测试与集成测试
│   ├── unit/
│   └── integration/
├── docs/                            # 完整文档（已在上方文档导航中列明）
├── package.json                     # 项目依赖与脚本定义
├── .env.example                     # 环境变量模板（端口、JWT 密钥、探测间隔）
└── README.md                        # 项目入口说明文件（即本文档）
```

## 贡献指南

1. **阅读行为准则与贡献守则**：请先查阅 `CODE_OF_CONDUCT.md` 与 `CONTRIBUTING.md` 文件，了解社区沟通规范、提交约定及签署 CLA（贡献者许可协议）的要求。

2. **选择或创建 Issue**：建议先在 Issues 列表中查找是否存在相关任务（新增资源、修复探测逻辑、优化文档等）。若无，请新建 Issue 并详细描述建议或问题，等待维护者确认。

3. **分支开发与本地测试**：从 `main` 分支切出功能分支（如 `feat/add-sports-source`），完成代码修改后，务必运行 `npm run test` 确保所有测试用例通过，并执行 `npm run build` 验证构建产物正常。

4. **提交变更与发起 Pull Request**：提交信息请遵循 [Conventional Commits](https://www.conventionalcommits.org/) 格式。PR 描述中需关联对应的 Issue 编号，并附上变更摘要与测试截图（若涉及界面改动）。

5. **审核与合并**：至少一名项目维护者会进行 Code Review，若通过则会合并至主分支，合并后 CI 将自动触发生产环境重新构建与部署。

## 常见问题

**Q：如何请求添加新的外部资源链接？**

A：您可以直接在 GitHub Issues 中提交带有 `[Resource Request]` 前缀的工单，并提供完整 URL、简要描述、所属分类及可用性验证证据（如 curl 输出）。社区审核通过后，将由维护者录入 `data/resources` 目录下的对应 JSON 文件中。若您具备开发能力，亦可按贡献指南直接发起 Pull Request。

**Q：平台如何处理收录链接的失效问题？**

A：每日定时任务会向所有收录的 URL 发送 HEAD 请求（部分站点支持 GET），记录响应状态、耗时及 TLS 信息。若连续三次探测均返回非 2xx/3xx 状态码，系统会将条目标记为 `degraded` 并在前端界面高亮提示。用户亦可手动触发即时探测以刷新状态。对于长期失效条目，社区将定期清理并归档至 `deprecated` 列表。

**Q：能否将本项目的资源数据同步到我的私有系统？**

A：可以。本项目提供只读的 JSON API 端点（`/api/v1/resources`），支持分页、过滤与字段选择。您也可以通过定时全量导出功能（`npm run export`）获得完整的 `resources.json` 文件，该文件采用 MIT 许可证发布，允许自由使用、修改与再分发，仅需保留原始版权声明。

## 许可证

MIT License

Copyright (c) 2026 DSZQ Community Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:05
