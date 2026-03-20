# AI 智能求职助手插件 — 产品方案说明文档

> **文档版本**：v1.0  
> **产品代号**：JobCopilot（暂定）  
> **文档状态**：草稿  
> **更新时间**：2026-03-20  

---

## 目录

1. [产品概述](#一产品概述)
2. [目标用户与核心痛点](#二目标用户与核心痛点)
3. [产品功能规格](#三产品功能规格)
4. [技术架构设计](#四技术架构设计)
5. [UI / UX 交互设计](#五ui--ux-交互设计)
6. [平台适配规范](#六平台适配规范)
7. [数据安全与合规设计](#七数据安全与合规设计)
8. [开发路线图](#八开发路线图)
9. [商业化设计](#九商业化设计)
10. [风险管理](#十风险管理)

---

## 一、产品概述

### 1.1 产品定位

**JobCopilot** 是一款基于 AI 大模型的浏览器扩展插件，专注于帮助国内求职者在 **BOSS 直聘、智联招聘、前程无忧、拉勾网、猎聘** 等主流招聘平台上实现：

- **智能岗位筛选**：依据用户简历内容自动分析每个职位的匹配度；
- **一键辅助投递**：在用户确认的前提下，辅助填写打招呼话术并提交，显著降低重复劳动；
- **求职进度管理**：统一记录已投递岗位、HR 回复状态及面试安排。

> 🎯 **核心理念**："AI 辅助（Copilot），人类决策（Human-in-the-Loop）"。插件不替代用户做决定，而是帮用户更快、更准地做决定。

### 1.2 与同类产品的差异化

| 维度 | 竞品（海外 LinkedIn Bot） | JobCopilot |
|------|--------------------------|------------|
| 平台覆盖 | LinkedIn / Glassdoor（海外）| BOSS/智联/前程/拉勾/猎聘（国内）|
| 操作模式 | 后台全自动盲投 | 前台半自动 + 用户确认 |
| AI 能力 | GPT 表单填写 | AI 匹配评分 + 定制话术生成 |
| 隐私保护 | 数据上传服务器 | 简历数据 100% 本地存储 |
| 封号风险 | 高（平台严打） | 低（拟人化操作 + 合规边界）|
| 用户门槛 | 高（Python 环境）| 低（一键安装 Chrome 扩展）|

---

## 二、目标用户与核心痛点

### 2.1 用户画像

#### 主要用户群：应届毕业生（秋招 / 春招）

- **人群特征**：在校生或刚毕业，投递高峰期（9月、3月），需要同时投递几十至上百家公司
- **核心痛点**：
  - 每天重复填写相同信息（姓名、学校、求职意向）极度耗时
  - 无法快速判断哪个岗位更适合自己
  - 投了哪些公司、状态如何，全靠手记或电子表格
- **付费意愿**：中等（学生群体，价格敏感）

#### 次要用户群：在职跳槽者（被动求职）

- **人群特征**：有工作但寻找更好机会，时间有限，无法频繁刷招聘软件
- **核心痛点**：
  - 想精准投递，不想海投浪费时间
  - 希望 AI 帮助判断岗位价值，只在"值得投"的岗位上花精力
  - 需要定制化打招呼话术，体现专业感
- **付费意愿**：高（有稳定收入）

#### 潜在用户群：求职培训机构 / 高校就业指导中心

- **需求**：批量辅导学生求职，需要工具提升效率
- **付费模式**：机构账号授权 / 教育版订阅

### 2.2 核心需求排序（MoSCoW 分析）

| 优先级 | 需求 |
|--------|------|
| **Must Have（必须有）** | 简历本地存储管理；JD 抓取与 AI 匹配评分；生成定制打招呼话术；辅助点击投递 |
| **Should Have（应该有）** | 投递记录面板；多平台支持；每日投递统计 |
| **Could Have（可以有）** | 基于 JD 的面试题生成；简历优化建议；岗位黑白名单管理 |
| **Won't Have（暂不做）** | 无人值守全自动批量投递（合规风险过高）|

---

## 三、产品功能规格

### 3.1 功能模块全览

```
JobCopilot 插件
│
├── 模块 1：简历管理中心 (Resume Manager)
│   ├── 支持上传 PDF / Word 简历，自动解析为结构化文本
│   ├── 支持手动编辑简历内容（富文本）
│   └── 支持保存多版本简历（如"前端版"/"产品版"）
│
├── 模块 2：AI 岗位分析引擎 (JD Analyzer)
│   ├── 实时抓取当前页面的岗位 JD 信息
│   ├── 调用 AI 接口，计算简历与 JD 的匹配分数（0-100）
│   ├── 输出匹配亮点（命中的技能/经历关键词）
│   └── 输出匹配缺口（JD 要求但简历中未体现的关键词）
│
├── 模块 3：智能话术生成器 (Greeting Generator)
│   ├── 根据匹配亮点自动生成 80~120 字的打招呼话术
│   ├── 话术风格可选（正式 / 轻松 / 突出技术 / 突出经验）
│   └── 支持用户自由修改后再使用
│
├── 模块 4：辅助投递执行器 (Apply Assistant)
│   ├── 高亮显示页面上的"立即沟通"/"投递简历"按钮
│   ├── 一键将生成的话术填入输入框（用户手动确认发送）
│   └── 自动滚动至下一条岗位（列表批量浏览模式）
│
└── 模块 5：求职进度追踪器 (Application Tracker)
    ├── 记录每次投递的岗位名称、公司、时间、匹配分数
    ├── 状态管理：已投递 → HR 已查看 → 已回复 → 面试中 → 已完成
    └── 数据导出为 Excel / CSV
```

### 3.2 功能详细说明

#### 功能 F01：简历解析与管理

**触发场景**：用户首次安装插件，进入 Options 设置页。

**输入**：
- PDF 文件上传（通过 `FileReader API` 读取）
- 直接粘贴简历文本

**处理逻辑**：
1. 调用本地 PDF.js 库解析 PDF，提取纯文本；
2. 调用 AI 接口，将原始文本结构化为 JSON 格式（见下方数据结构）；
3. 结构化结果存入 `chrome.storage.local`，不传输任何服务器。

**简历数据结构（JSON Schema）**：
```json
{
  "resume_id": "uuid-v4",
  "name": "张三",
  "version_label": "前端开发版",
  "contact": {
    "phone": "138****8888",
    "email": "zhangsan@example.com"
  },
  "education": [
    {
      "school": "XX大学",
      "degree": "本科",
      "major": "计算机科学",
      "graduation_year": "2024"
    }
  ],
  "experience": [
    {
      "company": "XX科技",
      "title": "前端开发实习生",
      "duration": "2023.07 - 2023.12",
      "description": "负责 Vue3 组件开发，优化页面加载速度30%..."
    }
  ],
  "skills": ["Vue3", "React", "TypeScript", "Node.js", "Git"],
  "raw_text": "原始简历全文...",
  "created_at": "2026-03-20T10:00:00Z"
}
```

---

#### 功能 F02：JD 自动抓取与 AI 匹配

**触发场景**：用户打开 BOSS 直聘、智联招聘等支持平台的某个职位详情页时，Content Script 自动触发。

**抓取目标字段**（以 BOSS 直聘为例）：

```javascript
// DOM 选择器配置（外部化 JSON，便于维护）
const BOSS_SELECTORS = {
  job_title:     ".job-name",
  company_name:  ".company-name",
  salary_range:  ".salary",
  job_tags:      ".job-tags .tag-item",  // 工作年限、学历、城市
  job_detail:    ".job-detail-section",  // 职位描述正文
  requirements:  ".job-requirements"
};
```

**AI 评分 Prompt 设计**：
```
系统角色：你是一位资深 HR，擅长评估候选人与岗位的匹配程度。

用户输入：
- 候选人简历摘要：{resume_summary}
- 岗位描述（JD）：{job_description}

输出要求（严格返回 JSON 格式）：
{
  "score": <0-100的整数，代表匹配度>,
  "level": <"高度匹配"|"基本匹配"|"勉强匹配"|"不匹配">,
  "matched_points": ["命中技能1", "命中经历2", ...],  // 最多5条
  "gap_points": ["缺少技能A", "经验不足B", ...],       // 最多3条
  "one_line_reason": "<50字内的核心理由>",
  "greeting": "<结合JD痛点，80-120字的专属打招呼话术，第一人称，自然口语化>"
}
```

**推荐 AI 接口**：
- **首选**：DeepSeek API（成本约 ¥0.001/千 tokens，中文理解最优）
- **备选**：Kimi（月之暗面）/ 阿里通义千问
- **隐私优先模式**：本地 Ollama（Qwen2.5 / DeepSeek-R1 量化版）

---

#### 功能 F03：辅助投递执行（合规设计的核心）

> ⚠️ **合规原则**：插件永远不会在没有用户明确触发的情况下自动发送任何内容。所有"投递"动作都由用户主动点击插件内的按钮触发，插件只负责**填写**和**高亮**，**发送由用户手动完成**。

**操作流程**（BOSS 直聘打招呼场景）：

```
用户操作：浏览职位列表，看到某个岗位
    ↓
插件自动：在列表卡片上叠加匹配分数气泡（如 "87分 ✓"）
    ↓
用户操作：点击岗位，进入详情页
    ↓
插件自动：侧边栏显示完整分析报告 + 打招呼话术
    ↓
用户操作：查看话术，按需修改后，点击插件按钮【填入对话框】
    ↓
插件执行：将话术文本注入"立即沟通"弹窗的输入框
    ↓
用户操作：检查内容后，手动点击平台的【发送】按钮
    ↓
插件记录：自动将该岗位加入投递记录，状态设为"已投递"
```

**关键合规设计**：
- 插件**不模拟点击**"发送"按钮，最后一步必须由用户完成；
- 每次填入话术前，弹出 Toast 提示："✅ 话术已填入，请检查后手动发送"；
- 不提供批量无人值守模式（全自动发送），消除平台封号风险；
- 操作间隔设置：用户从列表页点击下一条时，插件加入 `800ms~2000ms` 随机等待，避免机械性快速翻页。

---

#### 功能 F04：投递进度追踪面板

**页面设计**：点击插件图标打开 Popup，切换至"投递记录"标签页。

**状态流转**：
```
已投递 → HR已查看 → HR已回复 → 安排面试 → 面试完成 / 已拒绝 / 已录用
```

**数据存储位置**：`chrome.storage.local`（本地）

**记录字段**：
```json
{
  "record_id": "uuid",
  "platform": "boss",
  "job_title": "前端开发工程师",
  "company": "XX科技",
  "salary": "15-25K",
  "match_score": 87,
  "applied_at": "2026-03-20T14:30:00Z",
  "status": "hr_replied",
  "greeting_used": "您好，我看到贵司需要熟悉Vue3的前端...",
  "notes": "HR说本周会安排技术面",
  "resume_version": "前端开发版"
}
```

---

## 四、技术架构设计

### 4.1 整体架构图

```
┌──────────────────────────────────────────────────────────────────────┐
│                        浏览器环境                                      │
│                                                                        │
│  ┌─────────────────┐    ┌─────────────────┐    ┌──────────────────┐  │
│  │  Popup UI        │    │  Options Page    │    │  招聘平台网页      │  │
│  │  (React)         │    │  (React)         │    │  boss.zhipin.com │  │
│  │  - 投递记录面板  │    │  - 简历管理      │    │  zhaopin.com     │  │
│  │  - 今日统计      │    │  - API Key 配置  │    │  liepin.com      │  │
│  │  - 设置快捷入口  │    │  - 偏好设置      │    │                  │  │
│  └────────┬─────────┘    └────────┬─────────┘    └────────┬─────────┘  │
│           │                       │                        │ DOM注入    │
│           └───────────────────────┼────────────────────────┘           │
│                                   │                    ↑               │
│                    ┌──────────────▼──────────┐         │               │
│                    │   Background Service     │         │               │
│                    │   Worker (SW)            │◄────────┘               │
│                    │   - 消息路由与协调        │  Content Script消息     │
│                    │   - LLM API 调用          │                        │
│                    │   - chrome.storage 管理  │                        │
│                    │   - 投递记录持久化        │                        │
│                    └──────────────┬──────────┘                        │
│                                   │ fetch                              │
└───────────────────────────────────┼──────────────────────────────────┘
                                    │ HTTPS
                    ┌───────────────▼──────────────┐
                    │        外部 AI 接口            │
                    │   DeepSeek / Kimi / Qwen API  │
                    │   （仅传输 JD文本+简历摘要）    │
                    └──────────────────────────────┘
```

### 4.2 目录结构

```
jobcopilot/
├── manifest.json                  # MV3 配置文件
├── package.json                   # 依赖管理
├── src/
│   ├── background/
│   │   ├── index.ts               # Service Worker 入口
│   │   ├── llm.service.ts         # LLM API 调用封装
│   │   ├── storage.service.ts     # chrome.storage 读写
│   │   └── message.handler.ts     # 消息路由处理
│   │
│   ├── content/
│   │   ├── index.ts               # Content Script 入口（各平台共享）
│   │   ├── platforms/
│   │   │   ├── boss.ts            # BOSS 直聘专属适配器
│   │   │   ├── zhaopin.ts         # 智联招聘适配器
│   │   │   ├── liepin.ts          # 猎聘适配器
│   │   │   └── lagou.ts           # 拉勾网适配器
│   │   ├── components/
│   │   │   ├── ScoreBadge.tsx     # 列表页匹配分数气泡
│   │   │   └── Sidebar.tsx        # 详情页侧边栏分析面板
│   │   └── utils/
│   │       ├── dom-helper.ts      # DOM 操作工具函数
│   │       └── inject.ts          # 向输入框注入文本
│   │
│   ├── popup/
│   │   ├── index.html             # Popup 入口 HTML
│   │   ├── App.tsx                # React 根组件
│   │   └── pages/
│   │       ├── Dashboard.tsx      # 今日统计首页
│   │       └── Tracker.tsx        # 投递记录面板
│   │
│   ├── options/
│   │   ├── index.html             # Options 入口 HTML
│   │   ├── App.tsx
│   │   └── pages/
│   │       ├── ResumeManager.tsx  # 简历管理页
│   │       ├── ApiSettings.tsx    # AI 接口设置
│   │       └── Preferences.tsx    # 用户偏好设置
│   │
│   └── shared/
│       ├── types.ts               # 全局 TypeScript 类型定义
│       ├── constants.ts           # 常量（平台域名映射等）
│       └── selectors.json         # 各平台 DOM 选择器配置
│
├── public/
│   └── icons/                     # 插件图标（16/48/128px）
└── dist/                          # 构建产物（gitignore）
```

### 4.3 核心技术选型

| 模块 | 技术选型 | 选型理由 |
|------|---------|---------|
| 插件框架 | **Plasmo** (v0.88+) | 支持 React + TypeScript + HMR，内置 MV3 兼容，大幅减少样板代码 |
| UI 框架 | React 18 + TailwindCSS | 生态成熟，TailwindCSS 适合插件内嵌样式隔离 |
| 状态管理 | Zustand | 轻量，与 `chrome.storage` 同步方便 |
| PDF 解析 | PDF.js (Mozilla) | 纯前端解析，无需服务器，官方维护 |
| 向量匹配（可选）| Transformers.js | 浏览器内运行 BERT 模型，实现离线语义匹配 |
| AI 接口 | DeepSeek Chat API | 性价比最高，中文理解强，响应速度快 |
| 打包工具 | (Plasmo 内置) | Plasmo 基于 Parcel，自动处理 MV3 资产 |
| 测试 | Vitest + Playwright | 单元测试 + E2E 自动化测试 |

### 4.4 关键技术实现——选择器动态配置机制

招聘平台频繁改版是最大的维护痛点。通过将 DOM 选择器配置外部化，实现"不发版更新"：

```json
// src/shared/selectors.json
{
  "boss": {
    "domain": "boss.zhipin.com",
    "version": "2026.03",
    "job_list": {
      "card": ".job-card-wrapper",
      "title": ".job-name",
      "salary": ".salary",
      "company": ".company-name"
    },
    "job_detail": {
      "title": ".job-name",
      "salary": ".salary",
      "tags": ".job-tags .tag-item",
      "description": ".job-detail-section",
      "chat_button": ".btn-startchat",
      "input_box": ".chat-input-box textarea"
    }
  },
  "zhaopin": {
    "domain": "www.zhaopin.com",
    "version": "2026.03",
    "job_list": { ... },
    "job_detail": { ... }
  }
}
```

> **升级方案**：将 `selectors.json` 托管在 CDN（如 GitHub Raw），插件启动时拉取最新版本，当平台改版时只需更新 CDN 文件，无需走 Chrome 商店审核流程。

### 4.5 消息通信协议

插件各组件间通过 `chrome.runtime.sendMessage` / `onMessage` 通信，统一消息格式：

```typescript
// shared/types.ts
interface PluginMessage {
  type: MessageType;
  payload: unknown;
  requestId?: string;  // 用于异步响应匹配
}

enum MessageType {
  ANALYZE_JD         = "ANALYZE_JD",           // 请求分析当前 JD
  ANALYZE_JD_RESULT  = "ANALYZE_JD_RESULT",    // 返回分析结果
  INJECT_GREETING    = "INJECT_GREETING",       // 请求注入话术
  RECORD_APPLICATION = "RECORD_APPLICATION",   // 记录一次投递
  GET_RESUME         = "GET_RESUME",            // 获取当前简历
}
```

---

## 五、UI / UX 交互设计

### 5.1 核心界面原型描述

#### 界面 1：岗位列表页 — 匹配分数气泡

```
┌────────────────────────────────────────────────────┐
│  前端开发工程师  ·  某某科技  ·  15-25K             │
│  北京 · 3-5年 · 本科                      [89分✓]  │ ← 气泡注入
│  Vue3 / React / TypeScript                          │
└────────────────────────────────────────────────────┘
┌────────────────────────────────────────────────────┐
│  Java 后端工程师  ·  某某互联网  ·  20-35K          │
│  上海 · 5年以上 · 本科                    [31分✗]  │ ← 不匹配红色
│  Spring Boot / Kafka / K8s                          │
└────────────────────────────────────────────────────┘
```

- **绿色气泡（≥70分）**：强烈推荐投递；
- **黄色气泡（40-69分）**：可以尝试；
- **红色气泡（<40分）**：不建议，可设置"自动折叠低匹配岗位"。

---

#### 界面 2：岗位详情页 — 侧边栏分析面板（Sidebar）

```
┌─────────────────────────────────┐
│  🤖 JobCopilot  [关闭 X]        │
├─────────────────────────────────┤
│  匹配度分析                      │
│  ████████████░░  89 / 100       │
│  ● 高度匹配 ✓                   │
├─────────────────────────────────┤
│  ✅ 你的优势（命中关键词）         │
│  · Vue3 ★  · TypeScript ★       │
│  · 3年工作经验 ★                 │
├─────────────────────────────────┤
│  ⚠️ 待补充（JD要求未体现）         │
│  · Webpack 性能优化经验          │
│  · 微前端架构经验                │
├─────────────────────────────────┤
│  💬 AI 打招呼话术                 │
│ ┌───────────────────────────────┐│
│ │您好！我有3年Vue3开发经验，    ││
│ │曾负责某平台从Vue2完整迁移至   ││
│ │Vue3，性能提升40%。看到贵司    ││
│ │招聘前端工程师，非常感兴趣！   ││
│ └───────────────────────────────┘│
│  [✏️ 编辑话术]                   │
│                                  │
│  [📋 填入对话框]  [💾 记录岗位]  │
│                                  │
│  ▶ 点击"立即沟通"后，话术将     │
│    自动填入，请手动点击发送       │
└─────────────────────────────────┘
```

---

#### 界面 3：Popup 弹窗 — 投递记录面板

```
┌────────────────────────────────────┐
│  🎯 JobCopilot          [设置 ⚙️]  │
├────────────────────────────────────┤
│  今日已辅助投递: 12 家             │
│  本周: 47 家  ·  总计: 156 家      │
├────────────────────────────────────┤
│  最近投递                          │
│                                    │
│  前端开发 · XX科技      [HR已查看] │
│  2026-03-20  89分  BOSS直聘        │
│  ─────────────────────────────     │
│  产品经理 · XX互联网    [待回复]   │
│  2026-03-20  72分  智联招聘        │
│  ─────────────────────────────     │
│  全栈工程师 · XX创业    [已拒绝]   │
│  2026-03-19  55分  拉勾网     [删] │
│                                    │
│  [📊 查看全部记录]  [📥 导出Excel] │
└────────────────────────────────────┘
```

---

#### 界面 4：Options 页 — 简历管理

```
┌────────────────────────────────────────────────────────┐
│  我的简历库                              [+ 新建简历]  │
├────────────────────────────────────────────────────────┤
│  ● 前端开发版（当前使用）             [编辑] [设为默认]│
│    技能：Vue3, React, TypeScript                        │
│    更新于：2026-03-15                                   │
│  ─────────────────────────────────────────────────     │
│  ○ 产品经理版                         [编辑] [删除]    │
│    技能：产品设计, PRD, 数据分析                        │
│    更新于：2026-03-01                                   │
├────────────────────────────────────────────────────────┤
│  AI 接口设置                                           │
│  服务商：[DeepSeek ▼]                                  │
│  API Key：[●●●●●●●●●●●●●●●●]  [测试连接]             │
│                                                        │
│  ⚠️ API Key 仅存储在本地，绝不上传至任何服务器          │
└────────────────────────────────────────────────────────┘
```

---

## 六、平台适配规范

### 6.1 支持平台优先级与适配要点

| 平台 | 优先级 | 投递方式 | 适配难点 |
|------|--------|---------|---------|
| **BOSS 直聘** (boss.zhipin.com) | P0 最高 | "立即沟通"对话 | 输入框为富文本编辑器，需模拟 `input` 事件 |
| **智联招聘** (zhaopin.com) | P0 最高 | 在线简历投递表单 | 表单字段多，需支持自动填写基础信息 |
| **前程无忧** (51job.com) | P1 高 | 投递简历按钮 | 部分岗位需上传文件，暂不支持 |
| **拉勾网** (lagou.com) | P1 高 | "立即沟通"对话 | 与 BOSS 类似，登录态检测较严 |
| **猎聘** (liepin.com) | P2 中 | "主动沟通"对话 | 精英岗位为主，话术要求更专业 |

### 6.2 BOSS 直聘适配详细方案

```typescript
// src/content/platforms/boss.ts

import { PlatformAdapter } from '../types';

export const BossAdapter: PlatformAdapter = {
  name: 'BOSS直聘',
  domain: 'boss.zhipin.com',
  
  // 判断当前页面类型
  detectPageType: (): 'list' | 'detail' | 'chat' | 'unknown' => {
    const url = window.location.href;
    if (url.includes('/job_detail/')) return 'detail';
    if (url.includes('/web/geek/job')) return 'list';
    return 'unknown';
  },
  
  // 从详情页抓取 JD 信息
  extractJobDetail: () => {
    return {
      title:       document.querySelector('.job-name')?.textContent?.trim(),
      company:     document.querySelector('.company-name')?.textContent?.trim(),
      salary:      document.querySelector('.salary')?.textContent?.trim(),
      tags:        [...document.querySelectorAll('.job-tags .tag-item')]
                     .map(el => el.textContent?.trim()),
      description: document.querySelector('.job-detail-section')?.textContent?.trim(),
    };
  },
  
  // 将话术注入 BOSS 直聘聊天输入框
  injectGreeting: async (text: string): Promise<boolean> => {
    // 等待聊天框打开
    const inputEl = await waitForElement('.chat-input-box [contenteditable]', 3000);
    if (!inputEl) return false;
    
    // 聚焦输入框
    inputEl.focus();
    
    // 清空已有内容
    document.execCommand('selectAll');
    document.execCommand('delete');
    
    // 模拟逐字输入（触发平台的 input 事件监听）
    for (const char of text) {
      document.execCommand('insertText', false, char);
      await sleep(Math.random() * 20 + 10);  // 10~30ms 随机间隔
    }
    
    return true;
  },
};
```

---

## 七、数据安全与合规设计

### 7.1 数据流向说明

```
用户简历数据
    │
    ▼ 仅存储于
chrome.storage.local（浏览器本地加密存储）
    │
    ├── 不传输：原始简历全文
    ├── 不传输：用户联系方式（手机/邮件）
    │
    └── 仅传输至 AI 接口（HTTPS）：
        · 简历技能关键词列表
        · 工作经历描述（脱敏）
        · 当前 JD 文本
```

### 7.2 隐私保护措施

1. **数据本地化**：所有用户数据（简历、投递记录）存储在 `chrome.storage.local`，浏览器本身对该区域提供加密保护；
2. **最小化传输**：向 AI API 发送的内容为"简历摘要"而非完整简历，自动过滤手机号、邮箱、身份证等 PII 信息；
3. **用户自备 API Key（BYOK）**：AI 接口直接从用户浏览器调用，开发者无法拦截数据；
4. **透明的隐私政策**：在 Chrome 商店页面和 Options 设置页显著位置展示《隐私政策》，说明数据不被收集。

### 7.3 平台合规边界（规则范围内的关键约束）

| 约束项 | 设计实现 |
|--------|---------|
| 不替代用户发送消息 | 仅自动"填入"，最后"发送"步骤必须由用户手动触发 |
| 不绕过平台验证码 | 遇到验证码时，插件暂停并提示用户手动完成 |
| 不伪造用户凭证 | 利用用户已登录的 Cookie 状态，不模拟登录 |
| 不批量机器人操作 | 不提供定时任务 / 无人值守全自动投递功能 |
| 遵守频率限制 | 操作间隔内置随机延迟（0.8~2秒），不超过正常人类操作速度 |

### 7.4 隐私政策模板要点

```markdown
隐私政策要点（需托管于独立网页，供 Chrome 商店审核）：

1. 本插件不收集、不存储、不出售任何用户个人信息。
2. 您的简历数据仅存储在您自己浏览器的本地存储中（chrome.storage.local）。
3. 仅在您主动发起"分析"操作时，插件会将脱敏的简历关键词和岗位描述
   发送至您配置的第三方 AI 服务接口（如 DeepSeek），该过程受 HTTPS 加密保护。
4. 您随时可以在插件设置页删除全部本地数据。
5. 本插件不在后台静默运行，不收集浏览历史。
```

---

## 八、开发路线图

### 8.1 里程碑规划

#### 🚀 Phase 1：MVP（第 1-2 周）— 验证核心价值

**目标**：证明"AI 评分"有价值，用户愿意使用

**功能范围**：
- [x] 基础插件框架（Plasmo + React + TypeScript）
- [x] Options 页：简历文本粘贴与本地存储
- [x] Content Script：BOSS 直聘详情页 JD 抓取
- [x] Sidebar 注入：显示匹配分数 + 命中/缺口关键词
- [x] DeepSeek API 集成（BYOK 模式）

**验收标准**：用户打开一个 BOSS 直聘职位详情页，5 秒内看到 AI 匹配分数

---

#### ✨ Phase 2：核心功能完善（第 3-4 周）— 提升留存

**目标**：让用户每天打开都有价值，建立使用习惯

**功能范围**：
- [ ] 话术生成 + 一键填入 BOSS 直聘聊天框
- [ ] 投递记录 Popup 面板（存储 + 展示）
- [ ] 列表页匹配分数气泡注入
- [ ] 智联招聘适配（第二平台）
- [ ] PDF 简历上传解析功能

**验收标准**：用户能完成"看职位 → AI 评分 → 填入话术 → 记录投递"全流程

---

#### 💰 Phase 3：商业化（第 5-8 周）— 首批付费用户

**目标**：实现正向现金流

**功能范围**：
- [ ] Pro 功能权益系统（本地加密授权码）
- [ ] 接入支付宝 / 微信支付（通过 Lemon Squeezy 或 Paddle 实现）
- [ ] 批量列表扫描模式（Pro 功能）
- [ ] 面试题生成模块（Pro 功能）
- [ ] 前程无忧、拉勾网适配

---

#### 🌱 Phase 4：生态扩展（第 9-16 周）— 用户增长

**目标**：构建竞争壁垒

**功能范围**：
- [ ] 岗位趋势分析（哪类职位响应率高）
- [ ] 简历针对 JD 的定向优化建议
- [ ] 多设备数据同步（可选云端）
- [ ] 猎聘适配 + 牛客网校招支持
- [ ] 团队版 / 机构版授权

---

### 8.2 技术债务管理

| 风险点 | 预案 |
|--------|------|
| 平台 DOM 选择器失效 | 选择器外部化 + CDN 热更新；建立选择器监控脚本每日自检 |
| AI API 调用超时 | 设置 10s 超时 + 降级提示："AI分析中，可先查看岗位描述" |
| chrome.storage 容量限制 | 本地存储上限 5MB，超过后自动归档老记录至本地文件下载 |
| MV3 Service Worker 存活时间限制 | 避免在 SW 中保持长连接；使用 `chrome.alarms` 保活 |

---

## 九、商业化设计

### 9.1 定价策略

#### 免费版（获客）
- 每日 AI 匹配分析：10 次
- 话术生成：5 次/天
- 投递记录：最近 30 条
- 支持平台：BOSS 直聘（1 个）

#### Pro 版（¥19.9/月 或 ¥99/年）
- AI 匹配分析：无限次
- 话术生成：无限次（含 3 种风格）
- 投递记录：无限条 + 数据导出
- 支持平台：全部 5 个平台
- 专属功能：面试题生成、简历优化建议
- 优先客服支持

#### 积分包（按需付费）
- ¥9.9 = 500 积分
- 每次 AI 分析扣 1 积分，话术生成扣 3 积分
- 适合偶发需求用户

### 9.2 增长策略

```
阶段 1（0-100 用户）：种子用户冷启动
  → 在牛客论坛、V站、豆瓣求职小组发布"我用AI插件一天投了50个岗位"的真实测评
  → 邀请 5-10 名求职者作为内测用户，提供永久 Pro

阶段 2（100-1000 用户）：内容营销 + 社区运营
  → B站发布"AI求职助手开发幕后"技术视频（工程师群体共鸣）
  → 小红书发布"求职神器测评"软文（应届生群体）
  → 对所有 GitHub Star 用户提供 1 个月免费 Pro

阶段 3（1000+ 用户）：口碑裂变
  → 邀请奖励：邀请 3 人注册解锁 Pro 7 天
  → 与高校就业办、求职培训课程合作（机构版分销）
  → 秋招/春招旺季（9月/3月）限时折扣活动
```

---

## 十、风险管理

### 10.1 风险矩阵

| 风险 | 发生概率 | 影响程度 | 应对措施 |
|------|---------|---------|---------|
| 平台封号（用户账号被限制） | 中 | 高 | 严格遵守 Human-in-the-Loop 原则；操作拟人化；文档明确告知用户风险 |
| Chrome 商店审核被拒 | 中 | 中 | 提前准备隐私政策页面；避免在商店描述中使用"自动化"等敏感词；预留 2 周审核时间 |
| 平台 DOM 改版导致功能失效 | 高 | 中 | 选择器外部化热更新；建立每日巡检脚本；社区用户反馈快速响应 |
| AI API 费用超预期 | 低 | 中 | BYOK 模式（用户自备 Key）转移成本；限制免费版调用频次 |
| 法律合规风险（平台起诉）| 低 | 高 | 始终强调"辅助用户"而非"代替用户"；不收集平台数据用于商业目的；咨询法律顾问 |

### 10.2 竞争风险应对

- **平台自建 AI 功能**：招聘平台自身推出 AI 功能时，插件可转型为"跨平台数据聚合"和"个人求职数据分析"差异化定位；
- **大型竞品入场**：提前在中文求职社区建立品牌认知度，依靠社区口碑构建护城河；
- **开源替代品**：商业版提供高质量技术支持、持续维护和云端同步功能，与免费开源版形成差异。

---

## 附录

### 附录 A：MVP 快速启动命令

```bash
# 环境要求：Node.js 20+, pnpm 8+

# 1. 初始化 Plasmo 项目
pnpm create plasmo jobcopilot --with-src

# 2. 进入项目目录
cd jobcopilot

# 3. 安装依赖
pnpm install
pnpm add zustand pdf-parse @types/chrome tailwindcss

# 4. 启动开发服务器（Chrome 热重载）
pnpm dev

# 5. 加载扩展
# 打开 Chrome → 扩展程序 → 开启开发者模式 → 加载已解压扩展 → 选择 build/chrome-mv3-dev 目录

# 6. 构建生产版本
pnpm build

# 7. 打包提交 Chrome 商店
pnpm package
```

### 附录 B：DeepSeek API 快速接入示例

```typescript
// src/background/llm.service.ts

interface AnalyzeResult {
  score: number;
  level: string;
  matched_points: string[];
  gap_points: string[];
  one_line_reason: string;
  greeting: string;
}

export async function analyzeMatch(
  resumeSummary: string,
  jobDescription: string,
  apiKey: string
): Promise<AnalyzeResult> {
  const prompt = `
你是一位资深 HR，请评估候选人与岗位的匹配程度。

候选人简历摘要：
${resumeSummary}

岗位描述：
${jobDescription}

请严格按照以下 JSON 格式返回（不要有任何多余文字）：
{
  "score": <0-100整数>,
  "level": "<高度匹配|基本匹配|勉强匹配|不匹配>",
  "matched_points": ["<命中点1>", "<命中点2>"],
  "gap_points": ["<不足点1>"],
  "one_line_reason": "<50字内核心理由>",
  "greeting": "<80-120字打招呼话术，自然口语化，结合JD痛点>"
}
  `.trim();

  const response = await fetch('https://api.deepseek.com/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${apiKey}`,
    },
    body: JSON.stringify({
      model: 'deepseek-chat',
      messages: [{ role: 'user', content: prompt }],
      response_format: { type: 'json_object' },
      temperature: 0.3,
    }),
  });

  const data = await response.json();
  return JSON.parse(data.choices[0].message.content) as AnalyzeResult;
}
```

### 附录 C：参考资料

| 资源 | 链接 | 用途 |
|------|------|------|
| Plasmo 框架文档 | https://docs.plasmo.com | Chrome 扩展开发框架 |
| Chrome MV3 文档 | https://developer.chrome.com/docs/extensions/mv3 | 官方扩展 API 参考 |
| DeepSeek API 文档 | https://platform.deepseek.com/api-docs | AI 接口集成 |
| PDF.js 文档 | https://mozilla.github.io/pdf.js | PDF 解析 |
| Chrome 商店开发者注册 | https://chrome.google.com/webstore/devconsole | 上架 Chrome 扩展（$5 一次性费用）|

---

*文档维护者：产品/研发团队 | 反馈邮件：请联系项目负责人*
