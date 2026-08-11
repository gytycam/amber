# NexusIndex

NexusIndex 是一个面向技术内容聚合与资源导航的开源项目，专注于高质量外链资源的结构化整理与快速检索。项目定位于为开发者、研究员及内容创作者提供一套可自托管的资源索引框架，解决信息分散、链接失效与检索效率低下的问题。通过标准化的元数据描述与分类体系，NexusIndex 将零散的 URL 转化为可维护、可扩展的知识图谱，适用于个人书签管理、团队知识库构建及公开资源站部署。

作为第 565/567 批资源整合计划的载体，NexusIndex 不仅提供静态链接列表，还包含一套轻量级的分类规则与更新日志机制，确保资源在长期运营中保持可追溯性与一致性。项目本身不依赖任何第三方服务，所有数据以纯文本形式存储，兼容常见的静态站点生成器，便于用户按需定制展示层。

## 功能概览

- **资源分类与标签化**：每个链接支持多级分类标签与简短描述，方便按主题或领域筛选。

- **链接状态检测**：内置可选的链接可达性检查脚本，定期输出失效报告，辅助维护者更新。

- **全文检索支持**：集成基于标题、描述和标签的简易全文搜索，无需外部搜索引擎。

- **版本化变更日志**：每次增删改操作均记录于变更日志文件，便于回溯资源演进历史。

- **多格式导出**：支持将资源列表导出为 JSON、CSV 或 HTML 表格，适应不同使用场景。

- **自定义分类模板**：允许用户定义分类映射规则，自动将新链接归类至预设目录。

- **静态站点生成适配**：项目结构兼容 Hugo、Jekyll 等静态站点生成器，可一键生成导航页面。

## 应用场景

- **个人技术书签管理**：开发者可将日常浏览的技术文档、工具站、API 参考等链接按主题整理，并通过本地搜索快速定位，替代浏览器书签的混乱状态。

- **团队内部知识导航**：技术团队可利用 NexusIndex 搭建内部开发资源门户，统一存放运维手册、设计规范、公共库地址，减少重复询问与信息查找时间。

- **公开资源聚合站点**：教育机构或开源社区可基于本项目构建垂直领域的资源导航站，例如 AI 工具集、编程学习路径、数据集索引，为访客提供结构化入口。

- **研究文献外部链接库**：科研人员可整理论文关联的数据集、代码仓库、预印本链接，配合版本日志追踪链接变更，确保论文中的外部引用始终可追溯。

## 快速开始

以下操作以 Linux/macOS 环境为例，Windows 用户可使用 WSL 或 Git Bash 执行。

```bash
# 克隆项目仓库
git clone https://github.com/nexusindex/nexusindex.git
cd nexusindex

# 安装依赖（Python 3.9+ 及 pip）
pip install -r requirements.txt

# 初始化资源数据库（生成示例分类与索引）
python scripts/init_db.py --sample

# 启动本地 Web 预览服务（默认端口 8080）
python scripts/serve.py --port 8080
```

执行完成后，在浏览器中访问 `http://localhost:8080` 即可查看资源导航界面。若需更新资源列表，直接编辑 `data/links.json` 文件后运行 `python scripts/rebuild.py` 重新生成索引。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 及以上 | 核心脚本运行环境，用于链接处理与本地服务 |
| pip | 22.0 及以上 | Python 包管理器，用于安装 requirements.txt 中的依赖 |
| Git | 2.30 及以上 | 用于克隆仓库及版本控制操作 |
| 操作系统 | Linux / macOS / Windows (WSL) | 跨平台支持，Windows 原生命令行未完全测试 |
| 网络连接 | 出站可达 | 链接检测功能需要访问外部资源 |
| 磁盘空间 | 50 MB 以上 | 存放资源数据、日志及静态文件 |
| 内存 | 512 MB 以上 | 本地预览服务及构建脚本的内存占用 |
| 浏览器 | 现代浏览器（Chrome/Firefox/Edge） | 用于预览静态导航页面 |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|----------|-----------|
| 用户指南 | `docs/user-guide.md` | 如何添加、删除、修改资源链接？分类标签如何定义？ |
| 运维手册 | `docs/ops-manual.md` | 如何配置链接检测周期？如何迁移服务器？如何备份数据？ |
| 开发参考 | `docs/dev-reference.md` | 项目目录结构详解、核心脚本 API、自定义分类规则的编写方式。 |
| 部署示例 | `docs/deployment-examples.md` | 如何使用 Nginx 反向代理、如何集成 GitHub Actions 自动更新、如何部署到 Vercel。 |
| 常见问题 | `docs/faq.md` | 检索结果不准确怎么办？链接检测超时如何调整？导出格式乱码如何解决？ |

## 资源列表

### 技术资料与工具

<code>guochanjiqingzipai.org.cn</code>

<code>zhongwenzimuzhifu.org.cn</code>

<code>tewutushipin.org.cn</code>

### 社区与内容平台

<code>jinmantiantang.org.cn</code>

<code>wuyeyiren.org.cn</code>

<code>wuyeshuangshuangshuang.org.cn</code>

<code>tingtingyiren.org.cn</code>

## 项目结构

```
nexusindex/
├── data/                           # 核心数据目录
│   ├── links.json                  # 主资源列表（分类、标签、描述、URL）
│   ├── categories.yaml             # 分类映射规则定义
│   └── change_log.md               # 资源变更历史记录
├── scripts/                        # 工具脚本集合
│   ├── init_db.py                  # 初始化示例数据与目录结构
│   ├── serve.py                    # 本地轻量级 HTTP 预览服务
│   ├── rebuild.py                  # 根据 links.json 重新生成索引文件
│   ├── check_links.py              # 并发检查链接可达性，输出失效列表
│   └── export_formats.py           # 导出为 JSON / CSV / HTML 格式
├── templates/                      # 静态页面模板（Jinja2 格式）
│   ├── index.html.j2               # 导航主页模板
│   ├── category.html.j2            # 分类视图模板
│   └── detail.html.j2              # 单个资源详情模板
├── static/                         # 静态资源文件
│   ├── css/
│   │   └── style.css               # 基础响应式样式
│   └── js/
│       └── search.js               # 前端全文检索逻辑（纯客户端）
├── docs/                           # 项目文档
│   ├── user-guide.md
│   ├── ops-manual.md
│   ├── dev-reference.md
│   ├── deployment-examples.md
│   └── faq.md
├── tests/                          # 单元测试与集成测试
│   ├── test_links.py               # 链接解析与校验测试
│   └── test_export.py              # 导出格式正确性测试
├── requirements.txt                # Python 依赖清单（Flask, PyYAML, requests）
├── .github/                        # GitHub 相关配置
│   └── workflows/
│       └── check_links_daily.yml   # 每日定时链接检测工作流
├── LICENSE                         # MIT 许可证文件
└── README.md                       # 本文件
```

## 贡献指南

1. 查阅 `docs/dev-reference.md` 了解项目整体架构与核心脚本的调用关系，确认您要修改的功能模块不属于已知待办事项。

2. 在 GitHub 上 Fork 本仓库，创建新分支，分支命名遵循 `feature/功能简述` 或 `fix/问题简述` 格式，避免直接在主分支上修改。

3. 修改或新增资源数据时，请同步更新 `data/change_log.md` 文件，记录变更原因、日期及影响范围，保持变更历史完整。

4. 提交代码前，运行 `tests/` 目录下的所有测试用例，确保原有功能未被破坏；若添加新功能，请补充对应的测试用例。

5. 发起 Pull Request 至主仓库的 `main` 分支，描述中请说明修改目的、涉及文件及测试结果，至少一名维护者审核通过后方可合并。

## 常见问题

**Q：资源列表中的链接检测结果不准确，部分可访问的链接被标记为失效，如何处理？**

A：链接检测依赖 `scripts/check_links.py` 中的超时设置和重试次数。您可以编辑该脚本顶部的 `TIMEOUT` 和 `RETRIES` 变量，适当增加超时时间（例如从 5 秒改为 10 秒）或重试次数（从 1 次改为 3 次）。若为特定域名频繁误报，可在 `config/ignore_patterns.txt` 中添加该域名的忽略规则。

**Q：本地预览服务启动后，页面显示的资源分类与 `links.json` 中的标签不一致，是什么原因？**

A：分类映射由 `data/categories.yaml` 控制，该文件定义了标签到显示分类的转换规则。请检查 `categories.yaml` 中是否包含您所使用的标签名称，若缺少，请按 `标签名: 分类名` 格式添加新条目，然后重新运行 `python scripts/rebuild.py` 更新索引。

**Q：如何迁移 NexusIndex 到另一台服务器，并保留所有历史数据？**

A：迁移时只需打包整个项目目录（包括 `data/` 和 `docs/` 下的所有文件），在新服务器上安装相同版本的 Python 及依赖，然后执行 `python scripts/rebuild.py` 重新生成静态页面即可。`change_log.md` 中的历史记录会随文件一同迁移，无需额外操作。

## 许可证

MIT License。允许自由使用、修改、分发，需保留原始版权声明。详见项目根目录下的 LICENSE 文件。

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:21
