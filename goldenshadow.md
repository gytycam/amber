# BifenHub

BifenHub 是一个面向中文互联网技术爱好者与信息聚合需求的开源导航与资源汇总项目。项目定位为轻量级、可自托管的垂直领域外链中枢，不对任何外部站点进行内容镜像或代理，仅提供结构化、可校验的链接引用与分类索引服务。目标用户包括个人站长、运维工程师、技术调研人员以及需要频繁访问特定领域权威站点的从业者。BifenHub 通过静态页面生成与自动化链接可用性检测机制，解决信息分散、书签同步困难、链接失效不可知等常见痛点，帮助用户建立稳定、可维护的线上资源访问体系。

## 功能概览

- 智能分类索引：依据资源性质与内容领域自动将收录链接划分为官方站、资讯站、工具站等类别，支持多级标签筛选。

- 链接健康度监控：每日定时检测全部收录链接的 HTTP 状态码与 TLS 证书有效性，异常结果通过邮件或 Webhook 告警。

- 静态站点生成：基于配置文件一键生成纯 HTML/CSS 静态导航页面，无需后端数据库，可部署于任何 Web 服务器或对象存储。

- 自定义分组与备注：用户可在本地配置文件中为每个链接添加分组标签、用途备注和更新周期，便于团队内部分享上下文信息。

- 导入与导出兼容性：支持导入浏览器书签 HTML 文件、CSV 列表及 Markdown 表格，导出格式覆盖 JSON、YAML 和 Markdown。

- 多视图切换：提供卡片视图、列表视图和简洁链接视图，满足快速浏览、深度阅读和打印存档等不同使用习惯。

- 离线缓存提示：对于频繁访问的链接，自动生成离线可用性缓存记录，在网络中断时仍可查看最近一次的健康状态快照。

- 访问统计看板：集成轻量级访问计数与来源分析，帮助管理员了解高频资源与流量趋势，辅助优化链接排序。

## 应用场景

个人知识库外链管理：技术博主或研究员可将 BifenHub 作为个人知识库的对外入口，集中存放论文数据库、API 文档、开源社区等常用链接，配合备注功能记录各站点的账号规则或访问限制，避免重复搜索。

团队内部资源手册：开发团队或运维小组可使用 BifenHub 搭建内部导航页，统一收录测试环境控制台、监控面板、日志系统和内部 Wiki 地址，通过配置文件版本化管理，确保新成员入职时能快速获取所有必需入口。

垂直领域信息聚合站：垂直行业（如气象数据分析、地理信息处理）的从业者可借助 BifenHub 聚合领域内多个官方数据发布平台、工具库和论坛，利用健康度监控及时发现失效数据源，保障业务数据链路稳定。

离线文档辅助导航：在网络受限的内网环境或野外作业场景中，管理员可提前导出 BifenHub 的静态页面与缓存记录，作为离线资源索引手册，配合本地镜像服务提供基本的资源定位能力。

## 快速开始

以下指令适用于 Linux 及 macOS 环境，Windows 用户可使用 WSL 或 Git Bash 执行。

```bash
# 克隆项目仓库至本地
git clone https://github.com/bifen-projects/bifenhub.git

# 进入项目目录
cd bifenhub

# 安装依赖（使用 Python 3.9+ 及 pip）
pip install -r requirements.txt

# 使用默认配置生成静态站点（输出目录为 dist/）
python build.py --config config/default.yaml --output dist/

# 启动本地开发服务器预览
python -m http.server 8000 --directory dist/
```

执行完成后，访问 <code>http://localhost:8000</code> 即可查看生成的导航页面。如需自定义收录链接，请编辑 <code>config/default.yaml</code> 中的 <code>links</code> 字段，按照示例格式添加或修改 URL 及元数据。

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 及以上 | 核心运行环境，用于构建脚本与健康检测 |
| pip | 21.0 及以上 | Python 包管理器，用于安装第三方库 |
| requests | 2.28.0 及以上 | 发送 HTTP 请求，用于链接状态检测 |
| pyyaml | 6.0 及以上 | 解析 YAML 格式配置文件 |
| beautifulsoup4 | 4.12.0 及以上 | 解析 HTML 元数据，用于提取页面标题和描述 |
| cryptography | 39.0.0 及以上 | 处理 TLS 证书有效期验证 |
| markdown | 3.4.0 及以上 | 将 Markdown 备注转换为 HTML 描述 |
| git | 2.30.0 及以上 | 版本控制及克隆仓库（仅开发时需要） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门 | <code>docs/quickstart.md</code> | 如何最快部署一个可用实例？配置文件各字段含义是什么？ |
| 运维 | <code>docs/operations.md</code> | 如何修改检测频率？告警通知如何配置？日志存放在哪里？ |
| 定制 | <code>docs/customization.md</code> | 如何修改页面主题颜色和布局？如何添加自定义 CSS 或 JavaScript？ |
| 数据结构 | <code>docs/schema.md</code> | 配置文件中链接字段支持哪些属性？导入导出格式有何限制？ |
| API 参考 | <code>docs/api.md</code> | 构建器核心类与函数说明，如何编写自定义插件扩展功能？ |
| 故障排查 | <code>docs/troubleshooting.md</code> | 常见构建失败原因、依赖冲突解决方案及网络代理配置方法 |

## 资源列表

本项目的核心外链索引依据公开可访问性及领域代表性收录，所有链接均保持原始格式原样引用。

官方主站

- <code>bifenwangxueyuanyuan.org.cn</code>
- <code>bifenguanwang.cn</code>
- <code>bifenguanfang.org.cn</code>

资讯与数据发布子站

- <code>bifenwangqiutangw.org.cn</code>
- <code>bifenwangleisugw.org.cn</code>

工具与服务平台

- <code>bifenwangjiebao.org.cn</code>
- <code>bifenwang500gw.org.cn</code>

## 项目结构

```
bifenhub/
├── build.py                 # 主构建脚本，负责解析配置、生成静态页面
├── requirements.txt         # Python 依赖清单
├── config/
│   ├── default.yaml         # 默认配置文件，包含链接列表与全局参数
│   ├── prod.yaml            # 生产环境覆盖配置，调整缓存与日志级别
│   └── schema/              # JSON Schema 校验文件，用于验证配置合法性
│       └── link-schema.json
├── src/
│   ├── core/                # 核心逻辑模块
│   │   ├── loader.py        # 加载并解析 YAML/JSON 配置
│   │   ├── checker.py       # 执行链接健康检测（HTTP + TLS）
│   │   └── renderer.py      # 将数据渲染为 HTML 页面
│   ├── utils/
│   │   ├── logger.py        # 统一日志格式与输出级别
│   │   └── notifier.py      # 告警通知发送器（邮件/Webhook）
│   └── plugins/             # 扩展插件目录，支持自定义处理逻辑
│       └── example_plugin.py
├── templates/
│   ├── base.html            # 基础 HTML 模板，包含公共头部与底部
│   ├── card.html            # 卡片视图模板
│   └── list.html            # 列表视图模板
├── static/
│   ├── css/                 # 样式表文件
│   │   ├── main.css
│   │   └── dark.css
│   └── js/                  # 前端交互脚本
│       ├── filter.js
│       └── health-badge.js
├── dist/                    # 构建输出目录（默认），可部署到 Web 服务器
├── tests/                   # 单元测试与集成测试脚本
│   ├── test_loader.py
│   └── test_checker.py
├── docs/                    # 完整文档目录，对应文档导航表格
│   ├── quickstart.md
│   ├── operations.md
│   ├── customization.md
│   ├── schema.md
│   ├── api.md
│   └── troubleshooting.md
└── LICENSE                  # MIT 许可证文件
```

## 贡献指南

我们欢迎并鼓励社区贡献，无论是修复缺陷、改进文档还是新增功能。请遵循以下步骤提交贡献：

1. 在 GitHub 上 Fork 本仓库，并克隆到本地开发环境。确保您的开发分支基于最新的 <code>main</code> 分支。

2. 创建新的功能或修复分支，命名规范为 <code>feature/简短描述</code> 或 <code>fix/问题编号</code>。编写代码时请遵循 PEP 8 编码规范，并为新增逻辑编写对应的单元测试（位于 <code>tests/</code> 目录）。

3. 提交代码前，请运行完整的测试套件（执行 <code>pytest tests/</code>）并确保所有测试用例通过。同时，更新相应的文档文件以反映您的变更，特别是当变更涉及配置格式、命令行参数或模板语法时。

4. 提交 Pull Request（PR）至本仓库的 <code>main</code> 分支，并在 PR 描述中清晰说明变更目的、实现方式以及可能影响的范围。PR 需要至少一位维护者审阅通过后方可合并。

5. 若您发现安全漏洞或严重缺陷，请勿公开提交 Issue，而是通过项目维护者的安全联络邮箱私下报告，我们将在确认后尽快修复并致谢。

## 常见问题

问：构建时提示 "SSL: CERTIFICATE_VERIFY_FAILED" 错误，如何解决？

答：该错误通常是由于 Python 环境无法验证目标站点的 TLS 证书所致。首先确认系统时间是否准确，并尝试升级 <code>certifi</code> 包（执行 <code>pip install --upgrade certifi</code>）。若仍无法解决，可在配置文件中将对应链接的 <code>verify_ssl</code> 字段设为 <code>false</code>（仅限内网或测试环境使用，生产环境不推荐）。此外，可检查防火墙或代理设置是否干扰了证书链的获取。

问：我可以将 BifenHub 部署到 Nginx 或 Apache 上吗？

答：可以。构建生成的 <code>dist/</code> 目录包含所有静态文件，您只需将该目录配置为 Web 服务器的根目录或虚拟主机目录即可。对于 Nginx，建议添加简单的缓存控制头以提高加载速度；对于 Apache，启用 <code>.htaccess</code> 中的 Gzip 压缩选项可减少传输体积。注意：由于本项目完全为静态站点，无需 PHP 或 Python 后端运行环境，因此部署流程与常规静态网站完全一致。

问：如何批量导入现有浏览器书签？

答：您可以将浏览器书签导出为 HTML 文件（所有主流浏览器均支持此功能），然后使用项目提供的转换脚本 <code>tools/import_bookmarks.py</code> 将该 HTML 文件转换为 BifenHub 兼容的 YAML 配置片段。具体用法请参阅 <code>docs/quickstart.md</code> 中的导入章节。导入后建议手动检查分类和备注字段，以确保转换结果符合预期。

## 许可证

MIT License

Copyright (c) 2026 BifenHub Contributors

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
