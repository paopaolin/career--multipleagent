# 🎯 职业规划多智能体协同系统

> Career Planning Multi-Agent Collaboration System — 由 1 个监督者 + 4 个专家智能体 + 5 个 MCP 数据服务器组成的 AI 职业规划平台

[![Agents](https://img.shields.io/badge/智能体-5个-blue)](#-智能体团队)
[![MCPs](https://img.shields.io/badge/MCP服务器-5个-green)](#-mcp-数据工具链)
[![Model](https://img.shields.io/badge/模型-Fable%20%2B%20Sonnet-purple)](#-智能体团队)

---

## 📖 目录

- [项目概述](#项目概述)
- [系统架构](#系统架构)
- [智能体团队](#智能体团队)
- [MCP 数据工具链](#mcp-数据工具链)
- [协同工作流](#协同工作流)
- [快速开始](#快速开始)
- [使用示例](#使用示例)
- [新用户指南](#新用户指南)
- [项目文件结构](#项目文件结构)
- [配置说明](#配置说明)
- [扩展计划](#扩展计划)

---

## 项目概述

本项目构建一个**多智能体协同的职业规划系统**。由监督者智能体（Supervisor）作为中央协调器，负责任务理解、路由和结果汇总；4 个专家智能体各司其职，并行或顺序完成专业任务。

### 核心特点

- 🎯 **监督者协调**：Fable 模型驱动的中央协调器，智能理解用户意图
- 🔄 **并行分工**：多个专家智能体同时工作，大幅缩短等待时间
- 🧠 **持久记忆**：跨会话保存用户画像、职业目标、分析历史
- 🔌 **丰富数据源**：5 个 MCP 服务器覆盖 100 万+ 职位数据
- 🎨 **按角色分配模型**：Fable（监督者）+ Sonnet（专家），性能与成本最优平衡

---

## 系统架构

```
┌──────────────────────────────────────────────────────────────┐
│                     👤 用户交互层                              │
│              自然语言描述职业规划需求                           │
└─────────────────────────┬────────────────────────────────────┘
                          │
┌─────────────────────────▼────────────────────────────────────┐
│              🎯 监督者智能体 (Supervisor)                      │
│         模型: Fable | 意图理解 → 任务分解 → 结果汇总            │
│         记忆管理 | 质量把控 | 用户引导                          │
└──┬──────────┬──────────┬──────────┬──────────┬───────────────┘
   │          │          │          │          │
   ▼          ▼          ▼          ▼          ▼
┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────────┐
│🔍角色│ │🗺️学习│ │💼求职│ │🎤面试│ │🏢公司    │
│分析  │ │路径  │ │搜索  │ │准备  │ │调查      │
│专家  │ │规划  │ │专家  │ │专家  │ │(MCP)     │
│Sonnet│ │Sonnet│ │Sonnet│ │Sonnet│ │          │
└──┬───┘ └──┬───┘ └──┬───┘ └──┬───┘ └────┬─────┘
   │         │         │         │          │
┌──▼─────────▼─────────▼─────────▼──────────▼──────────────────┐
│                    🔧 数据工具层 (5个MCP服务器)                │
│  JobDataLake │ HireJack │ JD Intel │ LinkedIn Jobs │ Company Research │
│  1M+ 职位     │ 市场情报   │ ATS直搜   │ LinkedIn搜索  │ 公司深度调查     │
└──────────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────────┐
│                    🧠 状态与记忆层                              │
│  对话状态 (Context) │ 持久记忆 (Memory MCP) │ 用户画像 (本地文件) │
└──────────────────────────────────────────────────────────────┘
```

---

## 智能体团队

### 🎯 监督者 (Supervisor)
| 属性 | 值 |
|------|-----|
| **文件** | `.claude/agents/supervisor.md` |
| **模型** | Fable（最强协调能力） |
| **职责** | 意图理解、任务路由、结果汇总、记忆管理 |
| **权限** | 所有 MCP 工具 + Agent 工具 + Memory MCP |

### 🔍 角色分析专家 (Role Analyzer)
| 属性 | 值 |
|------|-----|
| **文件** | `.claude/agents/role-analyzer.md` |
| **模型** | Sonnet |
| **职责** | 技能盘点、JD 需求分析、差距评估、匹配度打分 |
| **输出** | 技能差距分析报告（含匹配度评分和优先级建议） |

### 🗺️ 学习路径规划专家 (Learning Path Creator)
| 属性 | 值 |
|------|-----|
| **文件** | `.claude/agents/learning-path-creator.md` |
| **模型** | Sonnet |
| **职责** | 基于差距生成学习路线、推荐资源、时间规划 |
| **输出** | 定制化学习路线图（含课程/书籍/项目推荐） |

### 💼 求职搜索专家 (Job Search Agent)
| 属性 | 值 |
|------|-----|
| **文件** | `.claude/agents/job-search-agent.md` |
| **模型** | Sonnet |
| **职责** | 多平台职位搜索、薪资分析、市场趋势、公司研究 |
| **数据源** | JobDataLake + HireJack + JD Intel + LinkedIn Jobs + Company Research |
| **输出** | 职位搜索报告（含匹配度排序和市场洞察） |

### 🎤 面试准备专家 (Interview Preparer)
| 属性 | 值 |
|------|-----|
| **文件** | `.claude/agents/interview-preparer.md` |
| **模型** | Sonnet |
| **职责** | 生成面试题库、回答框架、模拟脚本、准备清单 |
| **输出** | 面试准备包（含技术/行为/系统设计问题） |

---

## MCP 数据工具链

项目配置了 **5 个专用 MCP 服务器**，提供从职位搜索到公司调查的完整数据能力：

### JobDataLake — 主力搜索引擎
```
包名: @jobdatalake/mcp-server  v1.0.14
状态: ✅ 已验证（真实数据通过）
```

| 工具 | 功能 |
|------|------|
| `search_jobs` | 搜索 1M+ 职位（关键词/薪资/地点/技能/远程/经验） |
| `get_job` | 获取职位详情（描述、要求、薪资、投递链接） |
| `get_company` | 公司画像（行业、规模、招聘页） |
| `find_similar_jobs` | AI 向量相似度找相似职位 |
| `get_filter_options` | 获取可用过滤条件值 |

**免费额度**：500 次/天 | **覆盖率**：20,000+ 公司、40+ ATS 平台 | **更新频率**：每小时

### HireJack — 市场情报
```
包名: @hirejack/mcp  v0.1.6
状态: ✅ 已验证（真实数据通过）
```

| 工具 | 功能 |
|------|------|
| `search_jobs` | 科技岗位搜索（500+ 公司、80K+ 职位） |
| `get_company_profile` | 公司技术栈 + 薪资趋势 + 招聘分布 |
| `get_market_pulse` | 🔥 市场脉动：热门技能、热门雇主、远程占比 |
| `search_companies` | 按行业/名称搜索公司 |
| `compare_companies` | 多公司并行对比 |
| `get_market_history` | 市场历史趋势 |
| `find_breakout_companies` | 🚀 发现招聘猛增的公司 |
| `find_emerging_skills` | 🌱 发现新兴热门技能 |

**Pro 功能（需认证）**：技能差距分析、面试准备、简历重写、薪资基准、职位推荐

### JD Intel — ATS 直搜
```
包名: jd-intel-mcp  v0.8.1
状态: ✅ 已验证（真实数据通过）
```

| 工具 | 功能 |
|------|------|
| `fetch_jobs` | 搜索 Greenhouse/Lever/Ashby/SmartRecruiters/Teamtailor/Recruitee/Workday |
| `search_registry` | 查找公司使用的 ATS 平台 |
| `detect_ats` | 探测公司 ATS 类型 |

### LinkedIn Jobs — LinkedIn 职位
```
包名: linkedin-jobs-mcp-server  v1.0.0
状态: ✅ 已验证（注：需境外网络环境以绕过LinkedIn反爬）
```

| 工具 | 功能 |
|------|------|
| `search_linkedin_jobs` | LinkedIn 职位搜索（关键词/地点/薪资/远程/经验/日期过滤） |

### Company Research — 公司深度调查
```
位置: ./.company-research-mcp/  v1.0.0 (本地构建)
状态: ✅ 已验证 | 需要 Serper API Key (免费 2500次/月)
```

| 工具 | 功能 |
|------|------|
| `search_company_background` | 融资阶段、投资人、员工规模、增长轨迹 |
| `search_engineering_culture` | 技术博客、Glassdoor/Blind 评价、GitHub 开源情况 |
| `fetch_job_page` | 获取 JD 页面全文内容 |

---

## 协同工作流

### 完整规划流程（两阶段并行）

```
用户: "我是3年React前端，想转AI工程师，帮我规划"
                         │
                         ▼
              🎯 监督者：理解意图
              检查信息完整度 → 提问补充
                         │
          ┌──────────────┴──────────────┐
          │       阶段 1（并行）         │
          ▼              ▼              │
    🔍 角色分析      💼 求职搜索         │
    技能差距报告      市场职位搜索        │
          │              │              │
          └──────────────┴──────────────┘
                         │
          ┌──────────────┴──────────────┐
          │       阶段 2（并行）         │
          ▼              ▼              │
    🗺️ 学习路径      🎤 面试准备         │
    基于差距生成      基于JD生成          │
    学习路线图        面试材料            │
          │              │              │
          └──────────────┴──────────────┘
                         │
                         ▼
              🎯 监督者：汇总报告
              交叉验证 → 生成综合报告
              → 更新记忆系统
```

### 简单查询模式

```
"帮我搜索北京AI工程师岗位"  → 🎯 → 💼 求职搜索专家
"分析我和字节产品经理差距"  → 🎯 → 🔍 角色分析专家
"帮我准备蚂蚁金服面试"      → 🎯 → 🎤 面试准备专家
"推荐AI学习路线"            → 🎯 → 🗺️ 学习路径规划专家
```

---

## 快速开始

### 前置条件

- **Claude Code** 已安装
- **Node.js** >= 18

### 安装步骤

```bash
# 1. 进入项目目录
cd /Users/guanlin/Desktop/AI代码/agent部署

# 2. （可选）配置 Company Research MCP
# 注册 Serper API Key: https://serper.dev
cp .company-research-mcp/.env.example .company-research-mcp/.env
# 编辑 .env 填入 SERPER_API_KEY=你的key

# 3. 启动 Claude Code
claude
```

首次启动时，`.mcp.json` 中配置的 MCP 服务器会通过 `npx` 自动下载安装。

### 首次使用

```
# 在 Claude Code 中输入
根据以下信息帮我做职业规划：
- 我目前是XXX岗位，X年经验
- 技能：XXX, XXX, XXX
- 目标岗位：XXX
- 城市：北京
- 期望薪资：XXX
```

---

## 使用示例

### 示例 1：完整职业转型规划

```
用户:
我是3年前端开发，技术栈是React+TypeScript，
想转型AI工程师。目前在北京，期望薪资40K+。
帮我系统规划一下。

系统响应:
🎯 监督者启动完整流程 →
  → 🔍 角色分析：前端→AI技能差距（匹配度45%）
  → 💼 求职搜索：发现234个北京AI工程师岗位
  → 🗺️ 学习路线：12周学习计划（Python→ML→DL→项目）
  → 🎤 面试准备：针对Top 5公司生成面试包
  → 🎯 汇总：综合报告 + 优先级行动清单
```

### 示例 2：快速职位搜索

```
用户: 搜索上海的高级Java开发岗位，要有薪资信息的

系统响应:
💼 求职搜索专家 → JobDataLake search_jobs(
  query="Senior Java Developer",
  location="Shanghai",
  salary_min=200000,
  per_page=10
) → 返回匹配结果及薪资对比
```

---

## 新用户指南

> 详细版见 [📘 用户须知](./用户须知.md)

### 场景速览

| 你想做什么 | 这样说 | 系统会 |
|-----------|--------|--------|
| 🎯 完整规划 | "我是XX，想转YY，帮我规划" | 技能分析 + 职位搜索 + 学习路线 + 面试准备 → 综合报告 |
| 🔍 技能差距 | "分析我和XX岗位的差距" | 对比技能 vs JD 要求，给出匹配度评分 + 优先级建议 |
| 💼 找职位 | "搜北京30k以上的Go岗位" | 5 个数据源并行搜索，按匹配度排序，附薪资趋势 |
| 🗺️ 学习路线 | "帮我制定AI工程师学习计划" | 周级路线图 + 课程推荐 + 递进实战项目 |
| 🎤 面试准备 | "帮我准备腾讯后端的面试" | 技术题 + 行为题 + 系统设计题 + STAR 框架 |

### 你会得到的报告

```
📋 职业规划报告
 ├── 📊 现状评估    → 技能画像 + 优势 + 不足
 ├── 🎯 市场分析    → 匹配职位列表 + 薪资范围 + 热门技能
 ├── 🗺️ 学习路线    → 分阶段周计划 + 课程/书籍/项目推荐
 ├── 🎤 面试准备    → 分类题库 + 回答框架 + 模拟脚本
 └── 📋 行动建议    → 优先级排序的下一步行动清单
```

### 使用技巧

1. **描述越具体越好** — "我是3年React前端，熟悉TS和Node，想6个月内转AI工程师，base北京，期望35k+" 比 "帮我看看工作" 效果好得多
2. **首次做完整规划** — 让系统全面了解你，后续使用会更精准
3. **随时纠正** — 专家输出有偏差，直接告诉监督者，它会重新调度
4. **隔周搜一次** — 拿到学习路线后，定期搜索看市场变化

### 常见问题

<details>
<summary><b>需要付费吗？</b></summary>
完全免费。所有 MCP 数据源都有免费额度（每天 500 次搜索足够个人使用）。
</details>

<details>
<summary><b>我的数据安全吗？</b></summary>
所有用户数据仅保存在本地 <code>memory/</code> 目录，不会上传到任何第三方服务器。
</details>

<details>
<summary><b>搜索结果是实时的吗？</b></summary>
是的。JobDataLake 每小时更新，HireJack 和 JD Intel 实时抓取。
</details>

<details>
<summary><b>如果我只想问一个问题，也会启动全部专家吗？</b></summary>
不会。监督者会自动判断——简单查询只路由到对应专家，不会启动完整流程。
</details>

---

## 项目文件结构

```
agent部署/
├── setup.sh                           # 🔧 一键环境初始化脚本
├── 用户须知.md                         # 📘 新用户上手指南
├── index.html                         # 🌐 交互式流程可视化
├── CLAUDE.md                          # 项目上下文（Claude自动加载）
├── .mcp.json                          # MCP 服务器配置（5个服务器）
├── .claude/
│   ├── settings.json                  # 项目配置（权限/记忆）
│   ├── agents/
│   │   ├── supervisor.md              # 🎯 监督者智能体
│   │   ├── role-analyzer.md           # 🔍 角色分析专家
│   │   ├── learning-path-creator.md   # 🗺️ 学习路径规划专家
│   │   ├── job-search-agent.md        # 💼 求职搜索专家
│   │   └── interview-preparer.md      # 🎤 面试准备专家
│   └── skills/
│       └── career-planning.md         # 职业规划编排技能
├── .company-research-mcp/             # 🏢 本地 Company Research MCP
│   ├── package.json
│   ├── tsconfig.json
│   ├── .env.example                   # Serper API Key 配置模板
│   ├── src/
│   │   ├── index.ts                   # MCP 服务器入口
│   │   └── tools.ts                   # 工具实现
│   └── dist/                          # 编译输出
└── memory/
    ├── MEMORY.md                      # 记忆索引
    └── user-profile.md                # 用户画像模板
```

---

## 配置说明

### MCP 服务器总览

| 服务器 | 安装方式 | 认证需求 | 免费额度 | 核心数据 |
|--------|---------|----------|----------|----------|
| **JobDataLake** | npx 自动 | 无 | 500次/天 | 1M+ 职位 |
| **HireJack** | npx 自动 | 无（Pro需认证） | 公开工具免费 | 500+ 公司、80K+ 职位 |
| **JD Intel** | npx 自动 | 无 | 免费 | 7个ATS平台直搜 |
| **LinkedIn Jobs** | npx 自动 | 无 | 免费 | LinkedIn职位 |
| **Company Research** | 本地构建 | Serper API Key | 2500次/月 | Google搜索+网页抓取 |

### 环境变量

| 变量 | 用途 | 获取方式 |
|------|------|----------|
| `SERPER_API_KEY` | Company Research MCP 搜索能力 | https://serper.dev 免费注册 |

---

## 扩展计划

### 短期（1-2周）
- [ ] 接入 LinkedIn MCP OAuth 版本（支持档案访问和人脉管理）
- [ ] 添加简历解析智能体（PDF/DOCX → 结构化技能数据）
- [ ] JobDataLake 自定义 API Key 提升调用限额

### 中期（1-3月）
- [ ] 定时任务：每天自动搜索新职位并通知
- [ ] 学习进度追踪面板
- [ ] 面试模拟对话模式
- [ ] 薪资谈判辅助

### 长期（3-6月）
- [ ] 多用户支持
- [ ] Web 可视化面板
- [ ] 职位投递自动化
- [ ] AI 模拟面试官

---

## 技术栈

- **AI 模型**：Claude Fable 5 (监督者) + Claude Sonnet 5 (专家)
- **协议**：Model Context Protocol (MCP)
- **数据源**：JobDataLake、HireJack、JD Intel、LinkedIn Jobs、Serper API
- **记忆系统**：Memory MCP Server (知识图谱) + 本地 Markdown 文件

---

## 许可证

MIT

---

*Built with Claude Code Multi-Agent Framework | 2026年7月*
