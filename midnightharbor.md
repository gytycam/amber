# 足数通 · 足球数据与推荐资源聚合站

足数通（ZuqiuDST）是一个面向足球数据分析师、竞彩研究爱好者以及体育数据开发者的高质量外链与信息聚合开源项目。本项目不直接提供预测模型或投注建议，而是系统性地整理与足球赛事数据、历史统计、实时赔率、推荐算法研究相关的权威网络资源，解决足球数据研究领域信息分散、优质源难以发现、参考基线难以建立的核心痛点。

项目定位为“足球数据研究的导航台与基础设施”，目标用户包括独立数据科学家、量化投研团队、体育媒体数据编辑以及开源社区的数据工程学习者。通过本项目，用户能够以最低的时间成本接触到中文互联网环境下最具价值的足球数据源与推荐逻辑参考库。

## 功能概览

- **数据源分类索引**：按联赛、数据类型、更新频率对收录的 URL 进行多维度标签化组织，支持快速筛选。
- **外链健康度监测**：内置链接可用性检查脚本，定期输出各资源站点的响应状态与证书有效性报告。
- **推荐策略参考库**：收集并归档公开的足球推荐方法论、赔率转换算法及回测框架的说明文档。
- **数据标准化映射**：提供球队、联赛名称在各数据源间的别名映射表，降低多源数据合并的清洗成本。
- **历史快照对比**：对核心数据站点的公开历史页面进行定时快照与结构化差异对比，追踪数据修正轨迹。
- **社区规则镜像**：同步收录各数据平台的使用条款、机器人协议及访问频率限制说明，辅助合规采集设计。
- **自定义订阅生成器**：允许用户根据关注的联赛和统计指标，生成专属的资源订阅清单（JSON/OPML 格式）。

## 应用场景

- **量化模型特征工程**：数据科学家可通过本项目快速定位到提供球员跑动距离、传球成功率、射门转化率等细粒度统计的原始数据页面，用于构建比赛结果预测模型的特征集。
- **竞彩策略历史回测**：研究者在获取多个推荐源的公开历史推荐记录后，可对比不同推荐逻辑（如基于泊松分布、ELO 评级或市场情绪）的长期收益表现，建立自己的基准评估体系。
- **赛事数据仓库搭建**：数据工程师可依据本项目的资源列表，设计多源增量抽取管道（ETL），将分散的赛程、比分、伤病报告整合至统一的数据湖中，服务于球队管理或媒体内容生产。
- **开源数据工具验证**：当开发者新编写了足球数据爬虫或解析库时，可将本项目收录的站点作为目标测试集，验证解析器的鲁棒性与字段覆盖度。
- **足球数据分析教学**：高校教师或培训讲师可将本项目作为案例教学素材，引导学生理解真实世界中体育数据获取的渠道多样性及其质量评估方法。

## 快速开始

以下命令将完整克隆本项目至本地，并安装基础依赖（Python 3.9+ 及 requests、beautifulsoup4、pandas 库），最后运行初始资源可用性扫描。

```bash
# 克隆仓库
git clone https://github.com/zuqiudst/aggregator.git
cd aggregator

# 安装核心依赖
pip install -r requirements.txt

# 执行初始资源链接可用性检测（输出结果至 reports/ 目录）
python scripts/check_links.py --source data/resources.json --output reports/initial_scan.md
```

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 - 3.11 | 核心脚本运行环境，3.12 暂不支持部分异步库 |
| requests | >=2.28.0 | 发送 HTTP 请求以检测链接状态与重定向链 |
| beautifulsoup4 | >=4.11.0 | 解析 HTML 页面标题与 meta 描述，用于生成资源摘要 |
| pandas | >=1.5.0 | 维护数据源映射表及别名关联矩阵 |
| lxml | >=4.9.0 | 作为 beautifulsoup 的解析器后端，提高解析速度 |
| curl | >=7.68.0 | 部分高级检测脚本使用系统 curl 进行 TLS 指纹测试 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting_started.md | 如何理解本项目的分类体系、如何快速找到目标数据源、如何提交新资源 |
| 数据字典 | docs/data_dictionary.md | 资源 JSON 结构中各字段（如 update_freq、region、sport_type）的含义与取值范围 |
| 贡献规范 | docs/contribution_guidelines.md | 新增或更新 URL 时应遵循的格式、标签规则及验证流程 |
| 运维手册 | docs/operation_manual.md | 如何部署链接监控定时任务、如何处理失效链接、如何生成周期性报告 |

## 资源列表

本项目的核心资产为经过人工筛选与社区验证的足球数据及推荐相关网络资源。所有链接均按类别分组呈现，且严格保持用户提供的原始格式。

### 推荐类域名（主站与镜像）

- <code>zuqiudstuijian.net.cn</code>
- <code>zuqiudstuijian.cn</code>
- <code>zuqiudstuijian.com.cn</code>

### 数据类域名（主站与镜像）

- <code>zuqiudsshuju.net.cn</code>
- <code>zuqiudsshuju.org.cn</code>
- <code>zuqiudsshuju.cn</code>
- <code>zuqiudsshuju.com.cn</code>

## 项目结构

```
aggregator/
├── data/                                # 核心数据目录
│   ├── resources.json                   # 主资源列表（含所有 URL 及元数据）
│   ├── aliases/                         # 联赛/球队名称映射表
│   │   ├── premier_league_aliases.csv   # 英超别名对照
│   │   └── la_liga_aliases.csv          # 西甲别名对照
│   └── snapshots/                       # 历史页面快照存储区（按日期分目录）
├── scripts/                             # 可执行脚本
│   ├── check_links.py                   # 链接可用性扫描主程序
│   ├── snapshot_diff.py                 # 快照对比与变更通知
│   └── generate_subscription.py         # 自定义订阅清单生成器
├── docs/                                # 项目文档（详见文档导航）
│   ├── getting_started.md
│   ├── data_dictionary.md
│   ├── contribution_guidelines.md
│   └── operation_manual.md
├── tests/                               # 单元测试与集成测试
│   ├── test_parsers/                    # 各数据源解析测试用例
│   └── test_network/                    # 网络请求模拟与超时测试
├── reports/                             # 自动生成的扫描报告输出目录
│   └── weekly/                          # 周报存储
├── requirements.txt                     # Python 依赖声明
└── README.md                            # 本文件
```

## 贡献指南

1.  **Fork 本仓库并在本地开发环境中克隆**：通过 GitHub 的 Fork 功能将项目复制至你的账户下，随后使用 `git clone` 拉取至本地，并确保基于 `dev` 分支进行修改。
2.  **更新资源列表或文档**：若需新增 URL，请编辑 `data/resources.json`，严格按照数据字典填写所有必填字段（包括名称、分类、国家和更新频率）；若需完善文档，请修改 `docs/` 下对应的 Markdown 文件。
3.  **执行本地验证脚本**：在提交前，请于项目根目录运行 `python scripts/check_links.py --source data/resources.json --test-mode`，确保所有新增或修改的链接均返回有效的 HTTP 状态码（2xx 或 3xx）。
4.  **提交 Pull Request 至 dev 分支**：提交信息（Commit Message）应遵循语义化格式，例如 `feat: add new data source for Serie A` 或 `docs: update alias mapping for Bundesliga`。PR 描述中需明确说明变更动机及验证结果。
5.  **响应社区评审意见**：项目维护者会在 PR 中提出修改建议或合规审查问题，请在 5 个工作日内给予反馈或调整，否则 PR 将被关闭。

## 常见问题

**问：本项目是否提供直接的赔率数据或预测结果 API？**

答：不提供。本项目仅为外链导航与资源元数据聚合，不存储、代理或转发任何第三方数据内容。所有数据获取行为需用户自行与目标源站点建立连接，并遵守其服务条款。

**问：收录的某个链接无法访问或内容与描述不符，我该如何处理？**

答：请先在本地使用 curl 或浏览器确认问题现象，然后在本项目的 Issue 板块提交“链接失效”或“内容变更”报告，附上目标 URL、检测时间及错误截图。维护者将定期核实并更新资源列表或移除长期失效链接。

**问：我能否将本项目包含的资源列表用于商业产品中？**

答：本项目的资源列表（即 URL 集合及其分类标签）采用 MIT 许可证授权，允许自由使用、修改和再发布。但请注意，列表中指向的第三方网站各自拥有独立的版权与使用条款，你在访问或采集其数据时，需自行取得合法授权。

## 许可证

MIT License

Copyright (c) 2026 ZuqiuDST Aggregator Contributors

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
