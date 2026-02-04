# OpenClaw (ClawdBot) 拆解报告

> **阅读建议**：先看图表理解架构，再读文字深入细节。

---

## 一、系统架构图

```mermaid
flowchart TB
    subgraph "Messaging Channels"
        WA[WhatsApp]
        TG[Telegram]
        SL[Slack]
        IM[iMessage]
        DC[Discord]
        SG[Signal]
        EXT[Extensions<br/>Matrix/MSTeams/...]
    end

    subgraph "OpenClaw Gateway"
        GW[WebSocket Gateway<br/>Port 18789]
        CH[Channel Router]
        AG[Agent Runtime]
    end

    subgraph "AI Capabilities"
        SK[Skills System<br/>50+ Skills]
        AI[AI Models<br/>Pi/Coding-Agent/Oracle]
    end

    subgraph "Native Apps"
        IOS[iOS App<br/>Swift]
        MAC[macOS App<br/>Swift]
        AND[Android App<br/>Kotlin]
    end

    WA & TG & SL & IM & DC & SG & EXT --> CH
    CH --> GW
    GW --> AG
    AG --> SK
    SK --> AI
    GW <--> IOS
    GW <--> MAC
    GW <--> AND
```

**核心洞察**：所有消息渠道通过 **WebSocket 网关** 统一接入，而非分散的 webhook。这是架构上的关键决策。

---

## 二、Peter 的 Vibe-coding 三层模型

```mermaid
flowchart TB
    subgraph "Layer 1: Context"
        A[AGENTS.md]
        A_desc["<div align=left>
        • 项目结构映射<br/>
        • 命名规范<br/>
        • Git 工作流<br/>
        • 安全禁忌
        </div>"]
    end

    subgraph "Layer 2: Capability"
        S[SKILL.md]
        S_desc["<div align=left>
        • 能力定义<br/>
        • 使用场景<br/>
        • 输入/输出<br/>
        • 安全护栏
        </div>"]
    end

    subgraph "Layer 3: Process"
        W[Workflow]
        W_desc["<div align=left>
        • 决策点<br/>
        • 命令序列<br/>
        • 验证检查点<br/>
        • 故障恢复
        </div>"]
    end

    A --> S
    S --> W

    style A fill:#e1f5fe
    style S fill:#f3e5f5
    style W fill:#e8f5e9
```

**核心洞察**：不是让 AI 猜测，而是**分层提供上下文** → **声明能力** → **固化流程**。

---

## 三、Skills 系统架构

```mermaid
flowchart LR
    subgraph "Skill Sources"
        B[Bundled<br/>openclaw/skills/]
        M[Managed<br/>~/.openclaw/skills/]
        W[Workspace<br/>./skills/]
        P[Plugins<br/>extensions/*/]
    end

    L[Skill Loader]
    F[Filter by<br/:Environment]
    I[Inject to<br/>System Prompt]
    R[Runtime<br/>Execution]

    B & M & W & P --> L
    L --> F
    F --> I
    I --> R

    style R fill:#e8f5e9
```

### Skill 加载优先级

```
bundled → managed → workspace → extraDirs
(内置)    (用户级)   (项目级)    (额外目录)
```

---

## 四、SKILL.md 结构解析

```mermaid
flowchart TB
    subgraph "SKILL.md File"
        FM[YAML Frontmatter]
        BD[Markdown Body]

        subgraph "Frontmatter Fields"
            N[name]
            D[description]
            MD[metadata.openclaw]
        end

        subgraph "Body Sections"
            OV[Overview]
            QS[Quick Start]
            WF[Workflow]
            GD[Guardrails<br/>MUST/NEVER]
            EX[Examples]
        end
    end

    FM --> N & D & MD
    BD --> OV & QS & WF & GD & EX

    style FM fill:#e1f5fe
    style BD fill:#fff3e0
```

**示例**：`skills/github/SKILL.md`

```yaml
---
name: github
description: GitHub operations via CLI
metadata:
  openclaw:
    install:
      brew: ["gh"]
    requires: ["gh"]
---

## Overview
Operate GitHub via the gh CLI...

## Guardrails
- MUST: Use `gh pr view` before commenting
- NEVER: Edit workflow files directly
```

---

## 五、AI 工作流设计模式

```mermaid
flowchart TD
    START([Start]) --> A[Assess<br/>Divergence]
    A --> DEC{Decision<br/>Point}

    DEC -- Few local commits --> R[Rebase Strategy]
    DEC -- Many commits --> M[Merge Strategy]

    R -- Conflicts? --> RC{Resolve}
    RC -- Yes --> RF[Fix & Continue]
    RC -- No --> RB[Rebuild]

    M -- Conflicts? --> MC{Resolve}
    MC -- Yes --> MF[Fix & Commit]
    MC -- No --> MB[Rebuild]

    RF & RB --> RBUILD[Rebuild<br/>Everything]
    MF & MB --> MBUILD[Rebuild<br/>Everything]

    RBUILD & MBUILD --> PLAT[Platform<br/>Specific Build]
    PLAT --> V[Verify<br/>Checkpoints]
    V -- Pass --> END([End])
    V -- Fail --> ERR[Error<br/>Recovery]
    ERR --> PLAT

    style DEC fill:#fff3e0
    style V fill:#e8f5e9
    style ERR fill:#ffebee
```

**核心洞察**：每个工作流都有**决策门** + **验证检查点** + **故障恢复路径**。

---

## 六、多 Agent 安全模型

```mermaid
flowchart TB
    subgraph "Traditional Model"
        T1[Agent 1] --> T2[Git Repo]
        T3[Agent 2] --> T2
        T4[Conflict!] -.-> T2
    end

    subgraph "Peter's Model"
        subgraph "Safety Rules"
            R1[❌ No git stash]
            R2[❌ No worktree]
            R3[❌ No branch switch]
            R4[✅ Focus own edits]
        end

        A1[Agent 1] -- Follows Rules --> R[Git Repo]
        A2[Agent 2] -- Follows Rules --> R
    end

    style R1 fill:#ffebee
    style R2 fill:#ffebee
    style R3 fill:#ffebee
    style R4 fill:#e8f5e9
```

**核心洞察**：Peter 明确预设"多 Agent 同时工作"的场景，用规则避免冲突。这是大多数 AI 项目忽略的问题。

---

## 七、项目目录结构

```
openclaw/
├── src/
│   ├── gateway/          # WebSocket 网关（核心）
│   ├── channels/         # 消息渠道抽象
│   ├── agents/           # AI Agent 运行时
│   ├── cli/              # CLI 命令
│   └── [domain]/         # 领域分组
├── apps/
│   ├── ios/              # Swift iOS
│   ├── macos/            # Swift macOS
│   └── android/          # Kotlin Android
├── extensions/           # 插件（voice-call, msteams, matrix）
├── skills/               # 50+ 个内置技能
│   ├── github/SKILL.md
│   ├── slack/SKILL.md
│   └── ...
├── .agent/workflows/     # AI 工作流定义
└── AGENTS.md             # AI 运行手册（核心）
```

---

## 八、关键方法论总结

### 8.1 范式对比

```
传统开发:    人写代码 → 人写文档 → AI 辅助
Vibe-coding: 人写规范 → AI 写代码 → 人审验收
```

### 8.2 Peter 的核心原则

| 原则 | 实践 | 效果 |
|------|------|------|
| **预设多 Agent** | 禁止 stash/分支切换 | 避免协作冲突 |
| **上下文前置** | AGENTS.md 提供完整认知 | AI 不猜测 |
| **能力声明化** | SKILL.md 定义边界 | AI 知道能做什么 |
| **流程可执行** | Workflow 是命令序列 | 不只是文档 |
| **约定优于配置** | 目录/命名/流程约定化 | 降低决策成本 |

### 8.3 可立即借鉴的实践

1. **添加 AGENTS.md** —— 定义项目结构、规范、禁忌
2. **创建 SKILL.md** —— 用 YAML + Markdown 定义 AI 能力
3. **编写 Workflow** —— 为重复流程创建可执行手册
4. **设置安全规则** —— 明确禁止危险操作

---

## 九、核心文件速查

| 文件 | 用途 | 学习重点 |
|------|------|----------|
| `AGENTS.md` | AI 员工手册 | 多 Agent 安全规则 |
| `.agent/workflows/update_clawdbot.md` | 上游同步工作流 | 决策门 + 验证检查点模式 |
| `skills/github/SKILL.md` | 典型 Skill | YAML frontmatter 结构 |
| `skills/coding-agent/SKILL.md` | 复杂 Skill | Guardrails 写法 |

---

*拆解完成。图表优先，文字补充。*
