# BifenHub

BifenHub 是一个面向中文互联网技术内容聚合与导航的开源项目，定位为技术资料、在线工具与知识文档的外链资源中枢。项目本身不存储任何侵权或违规内容，仅通过结构化目录与人工筛选的 URL 集合，为开发者、研究人员与技术爱好者提供快速检索与主题分类的入口。本仓库旨在解决个人书签管理混乱、技术文档查找路径分散、以及优质外链资源难以长期稳定维护的痛点。

项目维护者定期对收录的链接进行存活检测与内容主题复核，确保资源列表的有效性与分类准确性。BifenHub 同时提供静态页面模板与自动化构建脚本，便于用户 fork 后搭建属于自己的技术导航站。

## 功能概览

- **按技术领域分类的资源索引**：将收录的 URL 按照后端开发、前端工程、运维监控、数据库、算法与竞赛等一级分类进行组织，每个分类下包含子标签与简短说明。

- **链接存活状态自动检测**：通过 GitHub Actions 定时任务（每周执行）对全部收录链接发起 HEAD 请求，标记异常状态码并生成状态报告。

- **Markdown 驱动的数据源**：所有外链数据以 Markdown 列表形式存储在 `_data` 目录下，用户可直接通过 PR 增删改，无需学习复杂数据库语法。

- **静态站点生成支持**：内置基于 Python 的简易生成器脚本，可将 Markdown 数据渲染为单页 HTML，支持暗色主题与响应式布局。

- **外链分类标签系统**：每条资源可标注多个主题标签（如 `#nginx` `#postgresql` `#algorithms`），支持在生成的导航页面中按标签过滤。

- **自定义书签导入导出**：提供脚本将浏览器书签 HTML 导出文件转换为本仓库的数据格式，也支持反向导出为 Chrome/Firefox 可识别的书签文件。

- **社区推荐机制**：通过 Issue 模板接收新的资源推荐，经维护者审核后合并入主库，推荐人可署名于资源注释中。

## 应用场景

- **技术团队内部知识库导航**：团队可将 BifenHub 作为基础框架，替换内部资源列表，统一团队成员查阅常用文档、内部工具与监控面板的入口，减少重复问询。

- **个人开发者工作流提效**：独立开发者或自由职业者可 fork 本仓库，按照自己的技术栈增删链接，配合本地静态生成器构建个人起始页，替代浏览器默认新标签页。

- **高校实验室或学生竞赛队伍资源整理**：适用于 ACM 竞赛、信息安全竞赛或机器人竞赛的队伍，用于集中存放常用 OJ 平台、论文检索入口、数据集下载站和工具手册。

- **技术社区文档共建**：开源社区或技术论坛可基于本仓库的数据格式开展“优秀外链收集”活动，由社区成员共同维护一份主题式资源清单，并部署为公开导航站。

## 快速开始

以下步骤适用于 Linux / macOS / WSL 环境，Python 3.9 及以上版本。

```bash
# 1. 克隆仓库
git clone https://github.com/your-org/bifenhub.git
cd bifenhub

# 2. 安装依赖（建议使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. 运行本地静态站点生成器
python build.py --watch

# 生成后的静态文件位于 _site 目录，可使用任何 HTTP 服务器提供访问
cd _site
python -m http.server 8080
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
| :--- | :--- | :--- |
| Python | 3.9 及以上 | 用于运行构建脚本与检测工具 |
| pip | 21.0 及以上 | Python 包管理工具 |
| requests | 2.28.0 及以上 | 链接存活检测的 HTTP 客户端库 |
| pyyaml | 6.0 及以上 | 用于解析可选的 YAML 配置文件 |
| markdown | 3.4.0 及以上 | 将数据描述渲染为 HTML 正文内容 |
| beautifulsoup4 | 4.12.0 及以上 | 解析现有 HTML 书签导入文件（仅导入功能需要） |

## 文档导航

| 层面 | 目录 / 资源 | 回答的问题 |
| :--- | :--- | :--- |
| 用户手册 | `docs/user-guide.md` | 如何使用已有导航数据、如何搜索资源、如何自定义本地生成样式 |
| 维护者指南 | `docs/maintainer-guide.md` | 如何审核新增链接、如何更新分类标签、如何处理失效链接的标记与移除 |
| 数据格式规范 | `docs/data-format.md` | 每条资源的 Markdown 字段定义（标题、URL、标签、备注、推荐人）、分类目录结构约定 |
| 构建与部署 | `docs/deployment.md` | 如何通过 GitHub Pages、Vercel 或 Cloudflare Pages 部署生成的静态站点，如何配置自定义域名 |

## 资源列表

本仓库收录的 URL 均来自社区推荐与人工筛选，按主题分类如下。

### 竞赛与成绩查询相关

- <code>danchaojifenbang.org.cn</code>
- <code>danchaobifen.org.cn</code>
- <code>bingdaochaobifen.net.cn</code>
- <code>bifenyingchao.org.cn</code>

### 数据与学术资源

- <code>bifenxueyuanyuan.org.cn</code>
- <code>bifenxijia.org.cn</code>
- <code>bifenwangxueyuanyuan.org.cn</code>

## 项目结构

```
bifenhub/
├── _data/                            # 核心数据目录
│   ├── competition/                  # 竞赛类资源列表（每行一个 Markdown 条目）
│   │   ├── score.md                  # 成绩查询相关链接（含 danchaojifenbang 等）
│   │   └── ranking.md                # 排名与榜单类链接
│   ├── academic/                     # 学术类资源（论文检索、数据集、预印本）
│   │   ├── databases.md              # 数据库与数据门户
│   │   └── journals.md               # 期刊与会议索引
│   └── navigation/                   # 综合导航与起始页资源
│       ├── homepages.md              # 个人主页与技术博客聚合
│       └── tools.md                  # 在线工具与 API 测试平台
├── scripts/                          # 工具脚本目录
│   ├── health_check.py               # 链接存活检测主脚本（被 Actions 调用）
│   ├── generate.py                   # 静态站点生成器核心逻辑
│   └── bookmark_convert.py           # 浏览器书签导入导出转换工具
├── assets/                           # 静态资源（CSS、字体、图标）
│   ├── css/                          # 响应式暗色主题样式
│   └── js/                           # 前端过滤与搜索交互脚本
├── templates/                        # HTML 模板（Jinja2 语法）
│   ├── base.html                     # 基础布局模板
│   └── index.html                    # 首页导航列表模板
├── docs/                             # 完整文档（用户手册、维护指南、格式规范、部署）
├── .github/                          # GitHub 相关配置
│   └── workflows/                    # Actions 工作流
│       └── health-check.yml          # 每周六 UTC 8:00 执行链接检测
├── requirements.txt                  # Python 依赖列表
├── build.py                          # 命令行入口脚本（封装生成与检测）
└── README.md                         # 本文件
```

## 贡献指南

1.  **提交 Issue 推荐新资源**：使用本仓库提供的 `New Resource Suggestion` Issue 模板，填写资源标题、完整 URL、建议分类和简短说明（50 字以内）。维护者会在 5 个工作日内回复审核意见。

2.  **直接提交 Pull Request 修改数据**：fork 本仓库后，在 `_data` 目录对应的分类文件中按已有格式添加或修改条目。请确保 URL 不包含跟踪参数（如 `?utm_source=...`），并检查链接可访问性。提交 PR 时请引用相关的 Issue 编号（如有）。

3.  **完善文档或翻译**：欢迎对 `docs/` 下的文档进行勘误、补充或翻译为其他语言。翻译请新增 `docs/zh-CN/` 以外的语言子目录，并保持与英文版内容同步。

4.  **报告失效链接或分类错误**：若发现资源列表中链接失效、跳转异常或分类不准确，请在 Issue 中选择 `Broken Link Report` 模板，提供失效 URL 与检测时间，维护者将及时处理。

5.  **本地构建与自测**：在提交 PR 前，请运行 `python build.py` 确保本地生成无报错，并检查 `_site` 目录下的输出页面是否正常显示新增或修改的内容。

## 常见问题

**Q: 为什么项目中只收录了少数域名，且看起来都集中在某个特定领域？**

A: BifenHub 目前处于第 518/567 批资源积累阶段，近期收录的链接主要来自社区内竞赛与成绩查询方向的热门推荐。项目路线图计划后续扩展至容器编排、云原生监控、数据库调优等更多技术子领域。欢迎通过贡献指南推荐您关注的优质资源。

**Q: 链接存活检测显示失败，但我手动访问是正常的，怎么处理？**

A: 检测脚本默认使用 HTTP HEAD 方法且不跟随某些重定向策略，部分站点可能拒绝 HEAD 请求或需要 Cookie 验证。若确认链接有效，可在资源条目的备注字段中添加 `#skip-check` 标签以跳过自动检测，或在该条目的元数据中设置 `check: false`。同时建议在 Issue 中告知维护者，以便调整检测策略。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:19
