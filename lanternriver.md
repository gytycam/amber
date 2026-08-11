# NovaIndex

NovaIndex 是一个面向技术社区与开源生态的轻量级资源导航与外部信息聚合系统。该项目定位于帮助开发者、研究人员与技术文档编写者高效整理、分类、检索与共享大量外部链接资源，解决信息分散、链接失效、分类混乱等常见问题。NovaIndex 以静态站点形式输出，支持 Markdown 驱动的数据源管理，适用于个人书签管理、团队知识库入口、项目文档外链附录等场景。项目本身不存储任何第三方内容，仅提供结构化的元数据描述与访问引导，严格遵循原始 URL 的完整性传递原则。

## 功能概览

- **链接分类与标签系统**：支持对任意外部 URL 进行多级分类与自由标签绑定，便于按主题、项目、优先级或状态筛选。
- **原始 URL 强制保真输出**：系统内部对用户输入的 URL 不做任何协议补全、大小写转换、结尾斜杠增删或域名改写，确保每个链接与输入时完全一致。
- **Markdown 驱动的数据源**：所有链接记录存储于纯文本 Markdown 文件中，支持版本控制、差异对比与协作编辑。
- **静态站点生成引擎**：内置模板引擎将数据源渲染为静态 HTML 页面，无需数据库，可部署于任何 Web 服务器或 CDN。
- **链接状态检测**：提供可选的定时任务模块，对已收录链接进行 HTTP 可达性检查，标注失效或重定向资源。
- **多维度检索**：支持按分类、标签、关键字全文搜索，以及按收录时间、点击频次排序。
- **导入与导出**：支持从浏览器书签 HTML 格式导入，以及导出为 JSON、CSV 或纯 URL 列表。
- **权限分级视图**：支持公共可见与内部受限两种发布模式，适用于开源项目外链和团队内部知识库双场景。

## 应用场景

- **开源项目外链附录**：在开源项目仓库中维护 `REFERENCES.md` 或 `RESOURCES.md` 文件，使用 NovaIndex 生成结构化的外部依赖、参考文献、工具站点索引，方便用户快速跳转。
- **技术团队内部知识入口**：技术团队可将常用开发文档、监控面板、CI/CD 链接、日志系统地址统一录入 NovaIndex，生成团队内部导航页，减少查找时间。
- **个人书签管理与备份**：个人开发者可将浏览器收藏夹导出后导入 NovaIndex，获得分类清晰、可搜索的私有书签站，同时保留原始 URL 完全不变，避免迁移时链接被篡改。
- **技术调研与竞品分析**：在进行技术选型或竞品调研时，将大量待分析站点录入 NovaIndex，按优先级、状态、归属公司等维度分类，辅助决策。
- **文档版本化外链治理**：企业文档团队可利用 NovaIndex 管理产品文档中的所有外部链接，当外部站点迁移时，只需在 NovaIndex 中更新一次，所有引用处同步生效。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/novaindex/novaindex.git

# 进入项目目录
cd novaindex

# 安装依赖（基于 Node.js 22 LTS）
npm install

# 构建静态站点（默认读取 ./data/links.md）
npm run build

# 启动本地预览服务器（默认端口 8080）
npm run serve
```

执行完成后，浏览器访问 `http://localhost:8080` 即可查看生成的导航站点。如需自定义数据源路径或输出目录，请参考 `config/default.yml` 中的配置项说明。

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Node.js | 22.x LTS 或更高 | 运行时环境，用于执行构建脚本与本地服务 |
| npm | 10.x 或更高 | 包管理器，用于安装项目依赖 |
| Git | 2.40 或更高 | 用于克隆仓库及版本控制 |
| 操作系统 | Linux / macOS / Windows（WSL2 推荐） | 跨平台支持，但建议 Unix-like 环境用于生产部署 |
| 内存 | 最低 512 MB，推荐 1 GB | 构建时内存占用随链接数量线性增长，约每万条链接增加 200 MB |
| 磁盘空间 | 最低 100 MB | 存放源码、依赖及生成静态文件，链接数量增加时仅输出文件体积增长 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide/` | 如何录入链接、如何分类、如何导入导出书签、如何配置搜索 |
| 管理员指南 | `/docs/admin-guide/` | 如何部署生产环境、如何配置链接状态检测、如何设置权限分级 |
| 开发者文档 | `/docs/developer-guide/` | 如何扩展模板引擎、如何自定义分类器、如何参与核心功能开发 |
| 设计原理 | `/docs/design-principles/` | 为什么强制保真原始 URL、为什么选择 Markdown 数据源、静态生成策略权衡 |

## 资源列表

本部分按类别整理项目收录的所有原始外部资源链接，每个链接均保留用户提供的原始形态，未做任何协议、域名或路径修改。

### 主域名类（裸域名）

- <code>danchaojishibifen.org.cn</code>
- <code>danchaojifenbang.org.cn</code>
- <code>danchaobifen.org.cn</code>

### 网络域名类（含 .net.cn）

- <code>bingdaochaobifen.net.cn</code>

### 子品牌或相关站点类（含二级域名）

- <code>bifenyingchao.org.cn</code>
- <code>bifenxueyuanyuan.org.cn</code>
- <code>bifenxijia.org.cn</code>

## 项目结构

```
novaindex/
├── src/                           # 核心源代码目录
│   ├── parser/                    # Markdown 数据源解析器
│   │   └── link-extractor.js      # 提取 URL、分类、标签及元数据
│   ├── generator/                 # 静态站点生成器
│   │   ├── html-renderer.js       # 模板渲染与页面组装
│   │   └── sitemap-builder.js     # 生成 sitemap.xml 供搜索引擎
│   ├── checker/                   # 链接状态检测模块
│   │   ├── http-client.js         # 轻量级 HTTP 请求封装
│   │   └── status-reporter.js     # 输出失效链接报表
│   ├── cli/                       # 命令行入口
│   │   └── index.js               # 暴露 build / serve / check 命令
│   └── utils/                     # 通用工具函数
│       ├── url-normalizer.js      # 仅做显示美化，不影响原始存储
│       └── logger.js              # 日志输出控制
├── data/                          # 用户数据目录
│   ├── links.md                   # 主链接数据源（Markdown 格式）
│   └── categories.yml             # 分类与标签映射配置
├── templates/                     # 页面模板文件
│   ├── layout.ejs                 # 全局布局模板
│   └── partials/                  # 可复用的头部、尾部、侧边栏组件
├── config/                        # 配置文件目录
│   ├── default.yml                # 默认配置（端口、输出路径、检测间隔）
│   └── production.yml             # 生产环境覆盖配置
├── dist/                          # 构建输出目录（自动生成）
│   ├── index.html                 # 首页
│   └── assets/                    # 静态资源（CSS、JS、图片）
├── tests/                         # 单元测试与集成测试
│   ├── parser.test.js             # 解析器测试用例
│   └── checker.test.js            # 状态检测测试用例
├── .gitignore                     # Git 忽略规则
├── package.json                   # 项目依赖与脚本声明
├── README.md                      # 项目说明文档（本文件）
└── LICENSE                        # MIT 许可证文本
```

## 贡献指南

1. 在 GitHub 上 fork 本仓库，并克隆到本地开发环境。建议使用 Node.js 22 及以上版本，并运行 `npm install` 安装全部开发依赖。
2. 创建新的功能分支，分支名请使用 `feature/` 或 `fix/` 前缀，后跟简要英文描述，例如 `feature/support-json-import`。
3. 进行代码修改或文档更新时，请确保新增或修改的功能包含对应的单元测试用例，且所有已有测试通过 `npm test` 命令。
4. 提交代码前运行 `npm run lint` 和 `npm run format` 以统一代码风格，并确保 `README.md` 中的资源列表保持与用户提供的原始顺序完全一致。
5. 提交 Pull Request 时，请清晰描述修改内容、影响范围以及是否涉及 URL 处理逻辑的变更，若涉及 URL 保真规则，需附上测试示例。

## 常见问题

**Q：为什么系统不允许对用户输入的 URL 进行任何自动补全或修正？**  
A：NovaIndex 的设计原则之一是“输入即输出”。自动补全协议（如将 `abc.com` 改为 `https://abc.com`）或强制统一大小写会导致某些内网地址、私有域名或特殊协议（如 `ftp://`、`telnet://`）无法正常工作。此外，许多外部站点对大小写敏感，强制改写会造成链接失效。因此项目从数据存储到渲染全程保留用户原始输入，仅在显示时做可选的美化提示，但实际跳转地址始终为原始值。

**Q：如何批量导入现有浏览器书签？**  
A：NovaIndex 提供了 `npm run import` 命令，支持从 Chrome、Firefox、Edge 导出的 HTML 书签文件直接导入。导入过程中系统会自动识别书签目录结构并映射为 NovaIndex 的分类层级，同时保留每个书签的原始 URL 不变。导入后可在 `data/links.md` 中查看并手动调整分类标签。

**Q：链接状态检测会影响现有数据吗？**  
A：不会。状态检测模块仅对配置的链接发起只读 HTTP HEAD 或 GET 请求，记录响应状态码和响应时间，并将结果写入独立的 `data/status-cache.json` 文件中。该缓存文件不会修改 `links.md` 主数据源，也不会影响已构建的静态页面，除非管理员主动执行 `npm run build -- --with-status` 将状态信息嵌入模板。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
