# Leisu Resource Navigator

Leisu Resource Navigator 是一个面向中文互联网技术内容聚合与导航的开源项目，旨在通过结构化、可维护的方式，整理和呈现特定垂直领域的高质量信息入口。项目定位为技术资源的外链汇总与元信息管理工具，不直接托管内容，而是提供清晰、可扩展的索引框架，帮助开发者、研究者与内容消费者快速定位有效信息源。

项目主要解决以下问题：中文网络环境中垂直领域信息分散、入口域名变动频繁、真伪难辨；技术爱好者缺乏统一、可信的起始导航点；自建信息聚合页维护成本高，难以版本化与协作。Leisu Resource Navigator 以开源仓库形式提供一套可 fork、可 PR 的导航数据与展示逻辑，让社区共同维护一份高质量的外部资源名录。

## 功能概览

- **多源入口索引**：整理并展示多个核心信息域名，按功能类别分组，支持快速跳转。

- **资源状态标记**：在文档与配置中标注每个入口的可用性、备案状态与推荐使用场景。

- **轻量级本地预览**：提供基于 Python 内置模块的静态 HTTP 服务器，一键启动本地导航页。

- **结构化数据存储**：所有外链信息以 YAML 与 JSON 格式存储于 `data/` 目录，便于脚本处理与版本对比。

- **自动生成 README 表格**：通过 GitHub Actions 或本地脚本，定期从数据文件生成最新的资源清单表格。

- **自定义重定向规则**：支持通过 `_redirects` 文件配置静态托管环境下的路径转发，适配域名迁移场景。

- **多格式导出**：支持将资源列表导出为 Markdown、CSV 与 HTML 片段，方便嵌入其他文档或博客。

- **变更日志跟踪**：每次 PR 合并前需更新 `CHANGELOG.md`，记录新增、失效或变更的资源入口。

## 应用场景

1. **个人技术起始页**：开发者可将本项目 fork 后，修改 `data/sources.yaml` 中的域名列表，替换为自己的常用技术文档、社区与工具站，作为浏览器新标签页的本地替代方案。

2. **社区文档协作**：技术社区或开源项目团队可使用本仓库作为外部链接白名单的公共编辑区，成员通过 PR 提交新的有用域名，经审核合并后统一发布。

3. **信息迁移与备份**：当原始服务域名发生变更时，维护者可通过 PR 批量更新 `data/` 下的映射关系，并利用 `_redirects` 实现旧路径到新路径的静默转发，降低外部书签失效影响。

4. **合规性审查辅助**：运维或法务人员可基于导出的 CSV 清单，快速对资源列表进行域名备案状态、ICP 许可证等维度的批量核验，输出审查报告。

## 快速开始

以下操作在 Linux / macOS / WSL2 环境下验证通过，Python 3.9+ 为必需依赖。

```bash
# 1. 克隆仓库
git clone https://github.com/your-org/leisu-resource-navigator.git
cd leisu-resource-navigator

# 2. 安装依赖（仅用于本地预览服务）
pip install --upgrade pip
pip install -r requirements.txt

# 3. 启动本地预览服务器，默认监听 8080 端口
python -m http.server 8080 --directory public

# 4. 或者使用提供的快捷脚本
./scripts/start_preview.sh
```

启动后，在浏览器中访问 `http://localhost:8080` 即可看到导航页。所有资源链接数据位于 `data/sources.yaml`，修改后刷新页面即可生效。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 或更高 | 用于运行本地预览服务器及数据处理脚本 |
| pip | 21.0 或更高 | Python 包管理工具，用于安装 requirements |
| Git | 2.30 或更高 | 用于克隆仓库及提交变更 |
| YAML 解析器 | PyYAML 5.4+ | 由 requirements.txt 自动安装，用于读取数据文件 |
| 静态 HTTP 服务器 | Python 内置 http.server | 无需额外安装，用于本地预览 |
| 可选：Docker | 20.10 或更高 | 如需容器化部署，用于构建镜像 |
| 可选：Node.js | 16.x 或更高 | 仅当使用前端构建脚本时需安装 |
| 磁盘空间 | 至少 50 MB | 用于存放仓库文件及缓存 |
| 网络 | 外网访问 | 用于首次 clone 及拉取依赖 |

## 文档导航

| 层面 | 目录 / 文件 | 回答的问题 |
|------|-------------|------------|
| 用户手册 | `docs/usage.md` | 如何添加自定义资源、如何导出清单、如何切换展示主题 |
| 维护者指南 | `docs/maintainers.md` | PR 审核流程、域名可用性检查方法、版本发布规范 |
| 数据格式参考 | `docs/data-format.md` | `sources.yaml` 中每个字段的含义、类型约束与示例 |
| 部署说明 | `docs/deployment.md` | 如何将项目部署到 Vercel、Netlify 或 GitHub Pages |
| API 参考 | `docs/api.md` | 提供的数据读取接口函数签名、参数与返回值说明 |
| 常见问题 | `docs/faq.md` | 汇集用户反馈的典型问题与解决方案 |
| 变更日志 | `CHANGELOG.md` | 每个版本新增、修改、废弃的资源条目记录 |

## 资源列表

### 核心推荐入口

<code>leisujinrituijian.org.cn</code>

<code>leisujinrituijian.cn</code>

### 实时比分与数据分析

<code>leisujishibifen.asia</code>

<code>leisujishibifen.cn</code>

### 专项分析服务

<code>leisufenxi.asia</code>

### 赛事结果查询

<code>leisubisaijieguo.asia</code>

### 综合比分信息

<code>leisubifenwang.asia</code>

## 项目结构

```
leisu-resource-navigator/
├── .github/                       # GitHub 相关配置
│   └── workflows/
│       └── build.yml              # 自动构建与部署流水线
├── data/                          # 核心数据目录（所有外链配置）
│   ├── sources.yaml               # 主资源列表（域名、分类、标签、状态）
│   ├── categories.yaml            # 分类元数据（显示名称、图标、排序权重）
│   └── redirects.json             # 路径重定向映射表
├── docs/                          # 完整文档目录
│   ├── usage.md                   # 用户使用手册
│   ├── maintainers.md             # 维护者操作规范
│   ├── data-format.md             # YAML 数据格式详细说明
│   ├── deployment.md              # 部署指南
│   ├── api.md                     # Python 辅助函数 API
│   └── faq.md                     # 常见问题汇总
├── public/                        # 静态资源根目录（预览服务入口）
│   ├── index.html                 # 导航页主 HTML
│   ├── style.css                  # 基础样式表
│   └── script.js                  # 前端交互逻辑（搜索、过滤）
├── scripts/                       # 工具脚本集合
│   ├── start_preview.sh           # 快速启动预览服务的 Shell 脚本
│   ├── export_csv.py              # 将 YAML 导出为 CSV 格式
│   └── check_availability.py      # 批量检查域名可达性的脚本
├── tests/                         # 单元测试与集成测试
│   ├── test_parser.py             # 测试 YAML 解析与数据校验
│   └── test_export.py             # 测试导出功能的正确性
├── requirements.txt               # Python 依赖清单（PyYAML, requests 等）
├── CHANGELOG.md                   # 版本变更历史记录
├── CONTRIBUTING.md                # 贡献者行为准则与操作指引
├── LICENSE                        # MIT 许可证全文
└── README.md                      # 项目总览（本文件）
```

## 贡献指南

1. 将本仓库 Fork 至个人账户，并克隆到本地。新建一个以 `feat/` 或 `fix/` 为前缀的分支，避免在主分支上直接修改。

2. 修改 `data/sources.yaml` 文件，按照已有格式添加、更新或移除资源条目。每个条目需包含 `name`、`url`、`category`、`status`（可用/待验证/失效）和 `description` 字段。

3. 本地运行 `python scripts/check_availability.py` 验证所有域名的基础可达性，并执行 `python -m pytest tests/` 确保现有单元测试全部通过。若新增分类，需同步更新 `data/categories.yaml`。

4. 提交变更时使用语义化提交信息，例如 `feat: add new analysis domain` 或 `fix: correct redirect mapping for score endpoint`。推送至远程分支后，在 GitHub 上创建 Pull Request，并在描述中说明变更目的与验证结果。

5. 等待维护者审核。如有修改意见，在 PR 评论区回复并推送更新。合并后，CI 流水线将自动生成新的导航页并部署至预览环境，同时更新本 README 中的资源列表表格。

## 常见问题

**Q: 我发现某个资源域名无法访问，应该如何处理？**

A: 请先自行通过浏览器或 `curl -I` 确认该域名的当前响应状态。如果确认失效，请在本仓库 Issues 中提交一个问题，标题注明 [Domain Down]，并提供域名、响应码及最后可访问日期。若你希望直接修复，可按贡献指南修改 `data/sources.yaml` 中对应条目的 `status` 为 `unavailable`，并提交 PR。

**Q: 能否添加非技术类或商业性质的外链？**

A: 本项目的定位是技术资源与信息导航，原则上仅收录与技术文档、开发工具、开源社区、学术文献、数据统计相关的域名。商业推广、电商、娱乐类链接不在收录范围。如有特殊需求，请在 Issue 中说明用途，由维护者评估。

**Q: 如何永久保存我自己定制的资源列表，而不与上游冲突？**

A: 推荐的做法是 Fork 本仓库后，将你的个性化配置提交到个人分支，并定期从上游的 `main` 分支合并更新。你也可以将 `data/sources.yaml` 复制到仓库根目录外的位置，在本地通过环境变量或命令行参数指定自定义数据文件路径，但这样将无法使用 GitHub Actions 的自动部署功能。

## 许可证

MIT License

Copyright (c) 2026 Leisu Resource Navigator Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
