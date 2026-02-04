# OpenClaw (ClawdBot) 深度拆解

> 本项目拆解分析：[openclaw](https://github.com/openclaw/openclaw) —— Peter 的 Vibe-coding 典范之作

---

## 一句话总结

OpenClaw 是**多平台消息网关 + AI Agent 运行时**：WebSocket 统一接入所有渠道 → Gateway 路由 → Agent 调用 Skills → 执行工具。

Peter 的 Vibe-coding 核心：**用结构化指令（AGENTS.md + SKILL.md + Workflow）替代代码注释，让 AI 成为真正的协作者。**

---

## 拆解文档索引

| 文档 | 内容 | 阅读建议 |
|:---|:---|:---|
| [📘 L1 主脉络](./openclaw-L1-主脉络.md) | 系统架构总览、数据流、模块关系 | **必读** — 先读这个 |
| [📗 L2-1 Gateway & Channels](./openclaw-L2-1-Gateway-Channels.md) | WebSocket 网关、渠道抽象层 | 理解接入层设计 |
| [📗 L2-2 Agent Runtime](./openclaw-L2-2-Agent-Runtime.md) | AI 对话引擎、会话管理 | 理解核心运行时 |
| [📗 L2-3 Skills System](./openclaw-L2-3-Skills-System.md) | 声明式能力扩展框架 | 理解可扩展性设计 |
| [📗 L2-4 Providers](./openclaw-L2-4-Providers.md) | 模型调度、多模型 Fallback | 理解 AI 集成层 |
| [📙 L3 细节实现](./openclaw-L3-细节实现.md) | 精确到函数和行号的代码速查 | 开发时参考 |

---

## Peter 的 Vibe-coding 方法论（提炼）

### 三层模型

```
Layer 1: Context    (AGENTS.md)    → 项目结构、规范、禁忌
Layer 2: Capability (SKILL.md)     → 能力定义、使用场景
Layer 3: Process    (Workflow)     → 决策点、命令序列、故障恢复
```

### 关键创新

| 创新点 | 说明 | 借鉴价值 |
|:---|:---|:---|
| **多 Agent 安全** | 禁止 stash/分支切换/工作树修改 | 避免多 AI 冲突 |
| **声明式 Skills** | Markdown + YAML 定义能力 | 无需改代码扩展功能 |
| **可执行工作流** | 提供命令序列而非描述 | AI 可直接执行 |
| **渐进式约束** | 约定 → 规范 → 能力 → 流程 | 降低认知负担 |

### 可立即落地的实践

1. **添加 AGENTS.md** —— AI 员工手册
2. **创建 SKILL.md** —— 声明式能力定义
3. **编写 Workflow** —— 固化重复流程
4. **设置安全规则** —— 明确禁止危险操作

---

## 技术架构亮点

| 设计 | 选择 | 原因 |
|:---|:---|:---|
| **统一接入** | WebSocket Gateway (端口 18789) | 避免各渠道 webhook 分散 |
| **渠道抽象** | Channel Registry 模式 | 新增渠道只需实现接口 |
| **能力扩展** | Skills System | 声明式定义，动态加载 |
| **模型调度** | Provider Dispatcher | 支持多模型 fallback |

---

## 核心文件速查

```
openclaw/
├── AGENTS.md                   # AI 员工手册（核心）
├── .agent/workflows/           # AI 工作流定义
├── skills/                     # 50+ 内置 Skills
│   └── */SKILL.md             # 能力定义文件
├── src/
│   ├── gateway/               # WebSocket 网关
│   ├── channels/              # 渠道抽象层
│   ├── agents/                # Agent 运行时
│   │   ├── skills/           # Skills 系统
│   │   ├── runtime.ts        # 运行时核心
│   │   └── session.ts        # 会话管理
│   └── providers/             # AI 模型调度
└── apps/                      # iOS/macOS/Android 原生应用
```

---

## 学习路径

### 路径 A：架构师视角
1. L1 主脉络 — 理解整体设计
2. L2-1 Gateway — 理解接入层
3. L2-3 Skills System — 理解扩展性
4. 总结方法论 — 应用到自己的项目

### 路径 B：开发者视角
1. L1 主脉络 — 快速浏览
2. L3 细节实现 — 找到具体代码位置
3. 对照源码 — 理解实现细节
4. 尝试修改 — 应用学到的模式

### 路径 C：方法论视角
1. 阅读本文档的"方法论提炼"部分
2. 查看 AGENTS.md 和 SKILL.md 示例
3. 在自己的项目中试点
4. 迭代优化

---

## 相关资源

- **OpenClaw 源码**: https://github.com/openclaw/openclaw
- **官方文档**: https://docs.openclaw.ai
- **本文档合集**: [case-studies/openclaw/](./)

---

*拆解完成。分层递进，图表优先，精确到行号。*
