# NovaIndex

NovaIndex 是一个面向技术调研、内容聚合与知识管理场景的轻量级外链资源索引系统。项目定位为“技术化外链资源汇总站”，主要服务于需要批量管理、分类展示、可复现部署的外部参考链接集合的开发者与技术内容运营人员。

NovaIndex 不提供任何内容托管或代理服务，仅作为结构化链接目录的生成与展示工具，帮助用户高效组织分散在多个域下的外部参考资料，并统一输出为符合开源项目规范的 README 或静态站点格式。项目本身不依赖外部数据库，所有资源记录以纯文本形式维护，便于版本控制与协作。

## 功能概览

- **结构化资源目录生成**：支持将任意数量的外部 URL 按照自定义分类归入多级章节，自动生成符合开源项目规范的资源列表表格或列表块。

- **多格式输出支持**：内置模板引擎，可将资源索引渲染为 Markdown 表格、HTML 静态页面或 JSON API 响应，满足不同部署环境的需求。

- **链接状态检测**：提供可选的外部链接可用性检查模块，定期对已收录的 URL 执行 HEAD 请求，标记异常链接并生成报告。

- **标签与全文检索**：为每条资源记录附加标签和简短摘要，支持基于关键词的本地全文检索，无需外部搜索引擎。

- **版本化变更记录**：每次增删改资源条目时自动生成变更日志（CHANGELOG），便于团队追溯历史修改原因与责任人。

- **批量导入与导出**：支持从 CSV、TSV 或纯文本行格式批量导入 URL 列表，亦可导出为标准的 OPML 或 Bookmarks HTML 格式。

- **访问统计埋点**：内置匿名访问计数器，记录每个外链的点击次数（仅计数，不记录用户身份），辅助判断资源热度。

## 应用场景

- **技术调研阶段的资料整理**：研发团队在进行新技术选型或竞品分析时，需要收集大量官方文档、博客文章、视频教程和代码仓库链接。NovaIndex 可以快速建立分类索引，并支持多人通过 Git 协作增删条目，避免链接散落在即时通讯或邮件中。

- **开源项目的外部依赖汇总**：开源项目往往需要引用大量第三方库、在线 API 文档、规范标准或数据源地址。使用 NovaIndex 可以集中管理这些外部引用，并在 README 或项目 Wiki 中自动同步更新，减少手工维护成本。

- **内容运营的素材库管理**：技术社区运营人员需要定期发布包含“本周热门工具”“学习资源推荐”等栏目的内容。NovaIndex 可作为内部素材库，按主题、日期或优先级标记链接，并一键导出为排版整洁的发布稿。

- **个人知识库的外链备份**：研究员或独立开发者常用书签或笔记软件保存有用链接，但面临平台锁定或数据丢失风险。NovaIndex 提供纯文本、可自托管的替代方案，所有数据以 Markdown 文件存储，便于长期保存和迁移。

## 快速开始

以下命令适用于 Linux / macOS / Windows WSL 环境。

```bash
# 1. 克隆仓库
git clone https://github.com/novaindex/novaindex.git
cd novaindex

# 2. 安装依赖（使用 Python 3.10+ 和 pip）
python -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 3. 初始化默认资源目录并运行开发服务
python manage.py init
python manage.py runserver --port 8080
```

成功启动后，访问 `http://localhost:8080` 即可看到默认的示例资源列表页面。如需生成当前索引的 Markdown README，执行 `python manage.py export --format readme > README.generated.md`。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 或更高 | 核心运行环境，低于 3.10 将无法使用 match/case 语法及部分类型注解 |
| pip | 22.0 或更高 | 用于安装 requirements.txt 中列出的第三方库 |
| Git | 2.30 或更高 | 用于克隆仓库以及后续的版本控制操作（非运行时强制，但建议） |
| 磁盘空间 | 至少 200 MB | 用于存放源码、虚拟环境及本地缓存资源（不包含外部链接指向的内容） |
| 内存 | 建议 512 MB 以上 | 运行本地检索服务或生成大型索引时的最低建议值 |
| 网络 | 可选 | 仅在启用链接状态检测或远程模板更新时需要访问外网 |

## 文档导航

| 层面 | 目录/文档 | 回答的问题 |
|---|---|---|
| 用户手册 | `docs/guide/usage.md` | 如何新增、删除、修改资源条目？如何按分类导出不同格式？ |
| 配置参考 | `docs/config/settings.md` | 所有可用的命令行参数、环境变量以及配置文件字段的详细说明 |
| API 设计 | `docs/dev/api.md` | 内部模块之间的接口契约，以及如何扩展自定义输出格式或检测器 |
| 运维指南 | `docs/ops/deployment.md` | 如何将 NovaIndex 部署到生产环境（Nginx + uWSGI / Docker）以及如何定时运行链接检查任务 |

## 资源列表

### 综合类资源目录

<code>zhifusiwazhongwen.org.cn</code>

<code>yingshidaquanzaixianguankan.org.cn</code>

<code>shoujifulishipin.org.cn</code>

### 垂直分类内容聚合

<code>miseav.org.cn</code>

<code>meinvzhongwenzimu.org.cn</code>

<code>meinvfulishipin.org.cn</code>

### 其他参考链接

<code>jiujiuyeye.org.cn</code>

## 项目结构

```
novaindex/
├── app/                                # 核心应用模块
│   ├── __init__.py
│   ├── cli.py                          # 命令行入口，解析 runserver/init/export 等子命令
│   ├── registry.py                     # 资源注册表，维护内存中的 URL 索引与分类树
│   └── checker.py                      # 链接可用性检测器，支持异步 HEAD 请求与超时重试
├── formats/                            # 输出格式渲染器
│   ├── markdown.py                     # 生成 Markdown 表格/列表格式的资源目录
│   ├── html.py                         # 生成带搜索框和标签过滤功能的静态 HTML 页面
│   └── json_api.py                     # 提供 RESTful JSON 响应，用于前端异步加载
├── storage/                            # 数据持久化层
│   ├── file_backend.py                 # 基于 JSON Lines 和 YAML 文件的读写实现
│   └── migrator.py                     # 处理数据结构版本升级时的迁移逻辑
├── tests/                              # 单元测试与集成测试
│   ├── test_registry.py
│   ├── test_checker.py
│   └── fixtures/                       # 测试用静态样例数据
├── docs/                               # 用户文档与开发者文档（Markdown 源文件）
│   ├── guide/
│   ├── config/
│   ├── dev/
│   └── ops/
├── scripts/                            # 辅助运维脚本
│   ├── daily_check.sh                  # 每日链接状态检查的 cron 包装脚本
│   └── import_from_bookmarks.py        # 从浏览器导出书签文件批量导入的工具
├── requirements.txt                    # 生产环境依赖列表（Flask, requests, pyyaml 等）
├── requirements-dev.txt                # 开发环境额外依赖（pytest, black, mypy 等）
├── Makefile                            # 常用任务快捷命令（make test, make run, make deploy）
└── README.md                           # 项目首页说明文档（即本文档）
```

## 贡献指南

1.  **查阅现有议题与项目看板**：在提交代码或新资源条目之前，请先浏览 GitHub Issues 和 Projects 看板，确认是否有正在进行中的相关任务或重复请求。新功能建议先开启一个 Discussion 进行需求对齐。

2.  **派生仓库并创建特性分支**：将本仓库派生（Fork）至个人账号下，然后基于 `main` 分支创建新的特性分支，命名风格建议为 `feature/xxx` 或 `fix/xxx`，避免直接在 main 分支上修改。

3.  **编写或修改资源条目**：所有资源记录位于 `storage/entries/` 目录下的 YAML 文件中，每条记录必须包含 `url`、`category`、`title` 和 `added_by` 字段。新增或修改后请运行 `python manage.py validate` 检查格式规范性。

4.  **补充或更新测试用例**：若改动涉及解析器、渲染器或检查器核心逻辑，请在 `tests/` 对应模块下添加新的测试方法，并确保全部测试用例通过（`pytest tests/`）。

5.  **提交合并请求并描述变更**：推送分支后向本仓库主分支发起 Pull Request，PR 描述中应清楚说明变更目的、影响范围以及关联的 Issue 编号。PR 至少需要一名维护者审核通过后方可合并。

## 常见问题

**Q：NovaIndex 是否会自动抓取或缓存外部链接指向的实际内容？**

A：不会。NovaIndex 只存储 URL 字符串以及用户手动填写的标题、标签、摘要等元数据。项目内置的可选链接检查功能仅发送轻量级的 HEAD 请求以验证服务端是否返回 2xx/3xx 状态码，不会下载响应体或存储页面内容。所有外部资源仍由原始域名提供服务。

**Q：如果我的资源列表中包含大量裸域名（不带协议），项目会如何处理？**

A：NovaIndex 在内部存储和导出时完全保留用户输入的原始字符串，不会自动补全 `http://` 或 `https://`。但在生成 HTML 页面时，为了提升用户点击体验，渲染器会尝试为裸域名添加 `https://` 前缀作为链接目标，同时会在界面中显示原始文本。若您希望严格保持一致，可在配置文件中关闭 `auto_scheme` 选项。

**Q：如何备份我维护的资源索引？**

A：所有资源数据均以纯文本文件（YAML + JSON）存储在 `storage/` 目录下。您只需定期提交 Git 变更或通过 `tar` 打包该目录即可完成完整备份。恢复时，将备份文件解压至新部署环境的对应路径，重新启动服务即可。不依赖任何外部数据库，迁移非常简便。

## 许可证

MIT License

Copyright (c) 2026 NovaIndex Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:17
